[UIWebView(基础)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIWebView(基础).md)

[UIWebView(进阶)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIWebView(进阶).md)

[UIWebView(高级)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIWebView(高级).md)

----------

看过前两篇博文的朋友，相信对UIWebView有一定的了解。但是有的地方不是很完善，今天是对UIWebView做一定的补充，实现的需求如下：

1. 运用苹果推出的JavaScriptCore实现JS和OC交互；
2. 升级进度条，能够更加精确的捕捉网页加载的进度。

>如果你还不知道JavaScriptCore库，详见我的博文《[JavaScriptCore框架](http://blog.csdn.net/y550918116j/article/details/49666443)》

#1 JavaScriptCore和UIWebView

在WWDC 2013上，苹果公司推出了JavaScriptCore框架，这是一个基于JavaScript的框架，它完美的以面向对象的方式实现js和oc的交互。

今天我们使用JavaScriptCore为大家介绍更优雅的js和oc交互。

##1.1 准备工作

这次创建的项目是借用前两篇博文的组合模式。完成后的界面图和进阶篇的截面图一样。

我们使用新的UIViewController，命名为SeniorVC。我已经为大家搭建了基础代码。

```objective-c
//
//  SeniorVC.m
//  UIWebView
//
//  Created by yangjun on 15/11/5.
//  Copyright © 2015年 六月. All rights reserved.
//

#import "SeniorVC.h"

@interface SeniorVC () <UIWebViewDelegate>

@property (weak, nonatomic) IBOutlet UIWebView *webView; ///< UIWebView
@property (weak, nonatomic) IBOutlet UIProgressView *progressView; ///< 进度条

@end

@implementation SeniorVC

- (void)viewDidLoad {
    [super viewDidLoad];

    self.webView.delegate = self;// 代理
    
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"index" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面

}

#pragma mark 刷新
- (IBAction)reload:(id)sender {
    NSLog(@"%d", self.webView.loading);
    // 正在刷新界面时，停止刷新后重新刷新界面
    if (self.webView.loading) {
        [self.webView stopLoading]; // 停止刷新
    }
    [self.webView reload]; // 刷新界面
}

#pragma mark 去上一页
- (IBAction)goBack:(id)sender {
    // 可以去上一页时，执行去上一页操作
    if (self.webView.canGoBack) {
        [self.webView goBack];
    }
}

#pragma mark 去下一页
- (IBAction)goForward:(id)sender {
    // 可以去下一页时，执行去下一页操作
    if (self.webView.canGoForward) {
        [self.webView goForward];
    }
}

#pragma mark - UIWebViewDelegate
#pragma mark 开始加载网页
- (void)webViewDidStartLoad:(UIWebView *)webView {
    NSLog(@"%@", NSStringFromSelector(_cmd));
}

#pragma mark 网页加载完成
- (void)webViewDidFinishLoad:(UIWebView *)webView {
    NSLog(@"%@", NSStringFromSelector(_cmd));
}

#pragma mark 网页加载出错
- (void)webView:(UIWebView *)webView didFailLoadWithError:(nullable NSError *)error {
    NSLog(@"%@:%@", NSStringFromSelector(_cmd), error.localizedDescription);
}

#pragma mark 网页监听
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    NSLog(@"%@", request.URL.absoluteString.stringByRemovingPercentEncoding);
    return YES;
}

@end
```

运行项目后你会看到如下效果图，如果你看不见，代表项目搭建有误。

![这里写图片描述](http://img.blog.csdn.net/20151106154733055)

##1.2 制造JavaScript协议

我们使用JSExport制造要暴露给JS调用的协议以及工具类JavaScriptUtil。

JavaScriptUtil.h

```objective-c
//
//  JavaScriptUtil.h
//  UIWebView
//
//  Created by yangjun on 15/11/6.
//  Copyright © 2015年 六月. All rights reserved.
//

#import <Foundation/Foundation.h>
#import <JavaScriptCore/JavaScriptCore.h>

/// 暴露给js使用的协议
@protocol JavaScriptDelegate <NSObject, JSExport>

- (void)jsCallOC:(NSString *)params;

@end

/// js对应的oc实现类
@interface JavaScriptUtil : NSObject <JavaScriptDelegate>

@property (nonatomic, weak) id<JavaScriptDelegate> javaScriptDelegate; ///< 代理

@end
```

这里使用了代理模式，思想就是JavaScript->JavaScriptUtil->SeniorVC。

>值得注意的是，在JavaScriptDelegate中禁止使用@optional属性。使用了这个属性后，js则无法调用。

JavaScriptUtil.m

```objective-c
//
//  JavaScriptUtil.m
//  UIWebView
//
//  Created by yangjun on 15/11/6.
//  Copyright © 2015年 六月. All rights reserved.
//

#import "JavaScriptUtil.h"

@implementation JavaScriptUtil

- (void)jsCallOC:(NSString *)params {
    if ([self.javaScriptDelegate respondsToSelector:@selector(jsCallOC:)]) {
        [self.javaScriptDelegate jsCallOC:params];
    }
}

@end
```

##1.3 实现交互

接下来，我们改造SeniorVC。

```objective-c
#import "SeniorVC.h"
#import <JavaScriptCore/JavaScriptCore.h>
#import "JavaScriptUtil.h"

@interface SeniorVC () <UIWebViewDelegate, JavaScriptDelegate>

@property (weak, nonatomic) IBOutlet UIWebView *webView; ///< UIWebView
@property (weak, nonatomic) IBOutlet UIProgressView *progressView; ///< 进度条
@property (nonatomic, strong) JSContext *jsContext; ///< JSContext

@end

@implementation SeniorVC

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    // 获取JSContext
    self.jsContext = [self.webView valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
    // 通过exceptionHandler捕获js错误
    self.jsContext.exceptionHandler = ^(JSContext *con, JSValue *exception) {
        NSLog(@"exception:%@", exception);
        con.exception = exception;
    };
    // 注入JavaScriptUtil
    JavaScriptUtil *jSUtil = [[JavaScriptUtil alloc] init];
    jSUtil.javaScriptDelegate = self;
    self.jsContext[@"app"] = jSUtil;
   
    self.webView.delegate = self;// 代理
    
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"Senior" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面

}

#pragma mark - JavaScriptDelegate
- (void)jsCallOC:(NSString *)params {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    NSString *js = [NSString stringWithFormat:@"ocCallJS('%@')", params];
    [self.jsContext evaluateScript:js];
    // 或
    // [self.webView stringByEvaluatingJavaScriptFromString:js];
}
@end
```

这里涉及到几个技术难点。

1. 使用documentView.webView.mainFrame.javaScriptContext可以从UIWebView中获取JSContext。
2. 使用JSContext的exceptionHandler监听js运行的错误；
3. 为JSContext注入JavaScriptUtil，并设为app。方便js直接调用。
4. 以SeniorVC指向JavaScriptUtil的代理，可在SeniorVC接受js发出的数据。

相信你也发现了我使用了一个新的html页面Senior.html。这个页面我们是使用的基础篇的index.html制造的。然后我们修改了里面核心js代码。

```js
<script>
    // $解析器
    function $ (ele) {
        return document.querySelector(ele);
    };

    // 点击确定按钮
    function onClickButton() {
        app.jsCallOC(input.value); // JS通知UIWebView
    }

    // UIWebView调用JS
    function ocCallJS(params) {
        show.innerHTML = params;
    }
</script>
```

其中app这个类型是通过`self.jsContext[@"app"] = jSUtil;`注入的。运行项目，则可在真机上看见神奇的交互效果。
&#160;

#2 进度条

在《[UIWebView进阶(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49638523)》中，我们是在回调内实现的进度条制造，但是这样精度不是很高。如果想要很高的精度，我们必然需要分析UIWebView的方法，也就是私有方法。这里可以使用runtime机制打印UIWebView方法。

>如果你不知道什么是runtime机制，详见我的博文《[Objective-C Runtime Classes](http://blog.csdn.net/y550918116j/article/details/48664237)》。

```objective-c
id userClass = objc_getClass("UIWebView");
u_int count;// unsigned int
Method *methods= class_copyMethodList(userClass, &count);// 所有方法,只包含实例方法）
for (int i = 0; i < count ; i++) {
    Method method = methods[i];
    SEL name = method_getName(method);// 转为方法
    const char *selName = sel_getName(name);// 转为方法名
    const char *methodTypeEncoding = method_getTypeEncoding(method);// 方法传输的参数
    char *methodType = method_copyReturnType(method);// 方法返回的类型
    fprintf(stdout, "方法名:%s; 返回类型%s; 参数:%s\n", selName, methodType, methodTypeEncoding);
}
```

分析打印出的方法，我们发现了这几个方法。

```objective-c
方法名:webView:identifierForInitialRequest:fromDataSource:; 返回类型@; 参数:@20@0:4@8@12@16
方法名:webView:resource:didFinishLoadingFromDataSource:; 返回类型v; 参数:v20@0:4@8@12@16
方法名:webView:resource:didFailLoadingWithError:fromDataSource:; 返回类型v; 参数:v24@0:4@8@12@16@20
方法名:webView:resource:didReceiveAuthenticationChallenge:fromDataSource:; 返回类型v; 参数:v24@0:4@8@12@16@20
方法名:webView:resource:didCancelAuthenticationChallenge:fromDataSource:; 返回类型v; 参数:v24@0:4@8@12@16@20
方法名:webView:resource:canAuthenticateAgainstProtectionSpace:forDataSource:; 返回类型c; 参数:c24@0:4@8@12@16@20
```

这几个方法很有意思，我就不一一介绍了，从字面就可以理解。虽然oc的方法很长，但当我们想做点比较私有的事时，还是很方便的。

##2.1 自定义UIWebView

接下来我们制造我们自己的UITableView，并可回调网络的加载进度，这里我使用的类名为ProgressWebView。

```objective-c
//
//  ProgressWebView.h
//  UIWebView
//
//  Created by yangjun on 15/11/6.
//  Copyright © 2015年 六月. All rights reserved.
//

#import <UIKit/UIKit.h>

@class ProgressWebView;

@protocol ProgressWebViewDelegate <UIWebViewDelegate>

@optional
/**
 *  接受到的数据
 *
 *  @param webView ProgressWebView
 *  @param receivedCount 总连接数
 *  @param totalCount    已完成连接数
 *
 *  @return void
 */
- (void)webView:(ProgressWebView *)webView didReceivedCount:(NSInteger)receivedCount totalCount:(NSInteger)totalCount;

@end

// 有进度的UIWebView
@interface ProgressWebView : UIWebView

@property (nonatomic) NSInteger totalCount;    ///< 总连接数
@property (nonatomic) NSInteger receivedCount; ///< 已完成连接数

@end
```

这里面有一个代理方法，和两个参数totalCount和receivedCount，这两个参数的主要作用是记录当前请求需要加载的资源数和已完成资源数。代理方法的主要作用是为了通知SeniorVC网络的加载情况。

接下来是ProgressWebView.m。

```objective-c
//
//  ProgressWebView.m
//  UIWebView
//
//  Created by yangjun on 15/11/6.
//  Copyright © 2015年 六月. All rights reserved.
//

#import "ProgressWebView.h"

// 暴露UIWebView的私有方法
@interface UIWebView()

- (id)webView:(id)webView identifierForInitialRequest:(id)initialRequest fromDataSource:(id)dataSource;
- (void)webView:(id)webView resource:(id)resource didFinishLoadingFromDataSource:(id)dataSource;

@end

@implementation ProgressWebView

#pragma mark 发出网络资源请求
- (id)webView:(id)webView identifierForInitialRequest:(id)initialRequest fromDataSource:(id)dataSource {
    self.totalCount++;
    return [super webView:webView identifierForInitialRequest:initialRequest fromDataSource:dataSource];
}

#pragma mark 网络资源获取成功
- (void)webView:(id)webView resource:(id)resource didFinishLoadingFromDataSource:(id)dataSource {
    [super webView:webView resource:resource didFinishLoadingFromDataSource:dataSource];
    self.receivedCount++;
    // 代理回调通知加载进度
    if ([self.delegate respondsToSelector:@selector(webView:didReceivedCount:totalCount:)]) {
        // 运用delegate类型转换为ProgressWebViewDelegate完成回调
        id<ProgressWebViewDelegate> pDelegate = (id<ProgressWebViewDelegate>)self.delegate;
        [pDelegate webView:self didReceivedCount:self.receivedCount totalCount:self.totalCount];
    }

}

@end
```

当你想调用私有方法时，为保证代码的完成性，需要使用super通知父类。这里我们就暴露了两个方法。

```objective-c
// 暴露UIWebView的私有方法
@interface UIWebView()

- (id)webView:(id)webView identifierForInitialRequest:(id)initialRequest fromDataSource:(id)dataSource;
- (void)webView:(id)webView resource:(id)resource didFinishLoadingFromDataSource:(id)dataSource;

@end
```

在回调的时候，由于UIWebView已经有了代理，为提高开发效率以及代码完成性，我们直接使用这个代理。只需向下转型即可。

```objective-c
id<ProgressWebViewDelegate> pDelegate = (id<ProgressWebViewDelegate>)self.delegate;
[pDelegate webView:self didReceivedCount:self.receivedCount totalCount:self.totalCount];
```

##2.2 改造SeniorVC

接下来就是改造我们的核心类SeniorVC，让你实现很酸爽的进度条。

先上代码，再详解。

```objective-c
//
//  SeniorVC.m
//  UIWebView
//
//  Created by yangjun on 15/11/5.
//  Copyright © 2015年 六月. All rights reserved.
//

#import "SeniorVC.h"
#import <JavaScriptCore/JavaScriptCore.h>
#import "JavaScriptUtil.h"
#import "ProgressWebView.h"

@interface SeniorVC () <UIWebViewDelegate, JavaScriptDelegate>
{
    CGFloat _startLoadingCount;  ///< 要加载的链接数
    CGFloat _finishLoadingCount; ///< 已加载的链接数
}

@property (weak, nonatomic) IBOutlet ProgressWebView *webView;     ///< UIWebView
@property (weak, nonatomic) IBOutlet UIProgressView *progressView; ///< 进度条
@property (nonatomic, strong) JSContext *jsContext;                ///< JSContext

@end

@implementation SeniorVC

- (void)viewDidLoad {
    [super viewDidLoad];
    // 获取JSContext
    self.jsContext = [self.webView valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
    // 通过exceptionHandler捕获js错误
    self.jsContext.exceptionHandler = ^(JSContext *con, JSValue *exception) {
        NSLog(@"exception:%@", exception);
        con.exception = exception;
    };
    // 注入JavaScriptUtil
    JavaScriptUtil *jSUtil = [[JavaScriptUtil alloc] init];
    jSUtil.javaScriptDelegate = self;
    self.jsContext[@"app"] = jSUtil;
   
    self.webView.delegate = self;// 代理
    
    // 1. JavaScriptCore交互
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"Senior" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
   // [self.webView loadRequest:urlRequest]; // 加载页面

    // 2.进度条
    url = [NSURL URLWithString:@"https://www.baidu.com"];
    urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面
    
}

#pragma mark 刷新
- (IBAction)reload:(id)sender {
    NSLog(@"%d", self.webView.loading);
    // 正在刷新界面时，停止刷新后重新刷新界面
    if (self.webView.loading) {
        [self.webView stopLoading]; // 停止刷新
    }
    [self.webView reload]; // 刷新界面
}

#pragma mark 去上一页
- (IBAction)goBack:(id)sender {
    // 可以去上一页时，执行去上一页操作
    if (self.webView.canGoBack) {
        _startLoadingCount = _finishLoadingCount = 0;
        [self.webView goBack];
    }
}

#pragma mark 去下一页
- (IBAction)goForward:(id)sender {
    // 可以去下一页时，执行去下一页操作
    if (self.webView.canGoForward) {
        _startLoadingCount = _finishLoadingCount = 0;
        [self.webView goForward];
    }
}

#pragma mark - JavaScriptDelegate
- (void)jsCallOC:(NSString *)params {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    NSString *js = [NSString stringWithFormat:@"ocCallJS('%@')", params];
    [self.jsContext evaluateScript:js];
    // 或
    // [self.webView stringByEvaluatingJavaScriptFromString:js];
}

#pragma mark - ProgressWebViewDelegate
- (void)webView:(ProgressWebView *)webView didReceivedCount:(NSInteger)receivedCount totalCount:(NSInteger)totalCount {
     // 网络加载时显示进度
    if ([UIApplication sharedApplication].networkActivityIndicatorVisible) {
        CGFloat progress = (CGFloat)receivedCount / totalCount;
        NSLog(@"%d:%d,%f", receivedCount, totalCount, progress);
        [self.progressView setProgress:progress animated:YES];
    } else {
        // 还原状态
        webView.receivedCount = 0;
        webView.totalCount = 0;
    }
}

#pragma mark - UIWebViewDelegate
#pragma mark 开始加载网页
- (void)webViewDidStartLoad:(UIWebView *)webView {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    // 开启网络加载提示
    [UIApplication sharedApplication].networkActivityIndicatorVisible = YES;
    if (_startLoadingCount == 0) {
        self.webView.receivedCount = 0;
        self.webView.totalCount = 0;
    }
    _startLoadingCount++;
}

#pragma mark 网页加载完成
- (void)webViewDidFinishLoad:(UIWebView *)webView {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    _finishLoadingCount++;
    // 获取document.readyState状态
    NSString *readyState = [webView stringByEvaluatingJavaScriptFromString:@"document.readyState"];
    BOOL complete = [readyState isEqualToString:@"complete"];
    if (complete && _finishLoadingCount == _startLoadingCount) {
        NSLog(@"加载完成");
        // 网页显示完毕时，初始化相关状态
        _startLoadingCount = _finishLoadingCount = 0;
        [self.progressView setProgress:1.0 animated:YES];
        // 关闭网络加载提示
        [UIApplication sharedApplication].networkActivityIndicatorVisible = NO;
    }
}

#pragma mark 网页加载出错
- (void)webView:(UIWebView *)webView didFailLoadWithError:(nullable NSError *)error {
    NSLog(@"%@:%@", NSStringFromSelector(_cmd), error.localizedDescription);
}

#pragma mark 网页监听
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    NSLog(@"%@", request.URL.absoluteString.stringByRemovingPercentEncoding);
    return YES;
}

@end
```

看代码，你可能感觉有点晕，我们主要做了以下几个操作。

1. 访问百度的链接www.baidu.com。
2. 实现JavaScriptDelegate的代理方法，并在代理里面改变UIProgressView的进度。
3. 使用进阶篇的进度条模式，这里做网页结束的控制。当然你也可以不这样使用，考虑到更高的精度，我这里这样使用了。
4. 使用网络的请求状态[UIApplication sharedApplication].networkActivityIndicatorVisible给用户友好提示。

运行代码后，你会看到很酷炫的进度条演示，并在控制台看见网页加载的详细进度。

![这里写图片描述](http://img.blog.csdn.net/20151106154720209)

&#160;

----------

#其他

##参考资料

[UIWebView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIWebView_Class/index.html)

[WWDC 2013: Integrating JavaScript into Native Apps](https://developer.apple.com/videos/play/wwdc2013-615/)

[iOS与JS交互实战篇（ObjC版）](http://mp.weixin.qq.com/s?__biz=MzIzMzA4NjA5Mw==&mid=214063688&idx=1&sn=903258ec2d3ae431b4d9ee55cb59ed89&scene=18#rd)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-6 | 运用苹果推出的JavaScriptCore实现JS和OC交互；升级进度条，能够更加精确的捕捉网页加载的进度。|
| 2015-12-11 | 更新相关博文链接 |
| 2015-12-12 | 更新博文名 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog