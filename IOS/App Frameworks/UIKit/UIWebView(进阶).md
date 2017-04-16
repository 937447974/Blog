[UIWebView(基础)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIWebView(基础).md)

[UIWebView(进阶)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIWebView(进阶).md)

[UIWebView(高级)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIWebView(高级).md)

----------

在上一篇博文我们已经讲了UIWebView的基础知识，这篇博文主要讲的知识点包含如下：

1. UIWebView设置
2. 清空浏览器的cookies；
3. 清除UIWebView的缓存；
4. 网页刷新；
5. 网页上一页和下一页；
6. 网页加载的进度条，效果如qq和微信的网页加载进度条。
&#160;

# 1 准备工作

## 1.1 搭建项目

前期的项目搭建和上一篇博文一样，只是在UINavigationController添加三个按钮。运行后的界面如图所示:

![这里写图片描述](http://img.blog.csdn.net/20151104131327196)

在图中有三个按钮，从左到右分别是刷新、后退和前进。

这里我使用的控制器为AdvancedVC。

```objective-c
//
//  AdvancedVC.m
//  UIWebView
//
//  Created by yangjun on 15/11/4.
//  Copyright © 2015年 六月. All rights reserved.
//

# import "AdvancedVC.h"

@interface AdvancedVC ()

@property (weak, nonatomic) IBOutlet UIWebView *webView; ///< UIWebView

@end

@implementation AdvancedVC

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
}

@end

```

## 1.2 显示网页

我们使用大家都熟悉的百度做测试，加载网页“https://www.baidu.com”。

```objective-c
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    NSURL *url = [NSURL URLWithString:@"https://www.baidu.com"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面
}

```

运行项目后，大家会发现一个错误。错误信息为

```
NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9802)
```

产生这个错误的原因是iOS9让所有的HTTP默认使用了HTTPS，原来的HTTP协议传输都改成TLS1.2协议进行传输。解决办法就是在项目的info.plist 文件里加上如下节点：

![这里写图片描述](http://img.blog.csdn.net/20151104132100861)

添加方式，找到info.plist如下方式打开。

![这里写图片描述](http://img.blog.csdn.net/20151104132113257)
&#160;

添加如下代码：
```
<key>NSAppTransportSecurity</key>
<dict>
	<key>NSAllowsArbitraryLoads</key>
	<true/>
</dict>
```

然后再次运行项目，则会看见百度的网页。

>这也能解决开发过程中，使用HTTP无法连接服务器的问题。

![这里写图片描述](http://img.blog.csdn.net/20151104152115623)

# 2 UIWebView设置

下面讲解在项目过程中关于UIWebView的常用设置。

## 2.1 背景设置

滑动屏幕到顶部时，再向上滑动，你会看到背景色，你可以通过backgroundColor自定义背景色。

```objective-c
self.webView.backgroundColor = [UIColor lightGrayColor]; // 设置背景色
```

## 2.2 页面滚动弹跳

上面说的，你可以滑动到顶部看到背景色，这里有一个弹跳效果。也就是你看见背景色，松开手指看到的效果。这里你可以禁用这种效果。

```objective-c
self.webView.scrollView.bounces = NO;
```

## 2.3 缩放

默认情况下UIWebView加载HTML页面后，会以页面的原始大小进行显示，亦即如果页面的大小超出UIWebView视口大小，UIWebView会出现滚动效果，而且用户只能通过滚动页面来查看不同区域的内容，不能使用手指的捏合手势来放大或缩小页面。通过设置


```objective-c
self.webView.scalesPageToFit = YES ;
```

UIWebView可以缩放HTML页面来适配其视口大小，从而达到整屏显示内容的效果，并且用户可以用捏合动作来放大或缩小页面来查看内容。
&#160;

# 3 清空cookies

清空cookies使用到了NSHTTPCookieStorage类，这个类主要用于管理网络中的cookies。

在viewDidLoad方法中添加如下代码,这里我们清空了所有cookies。

```objective-c
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    NSLog(@"清除cookies");
    NSHTTPCookieStorage *storage = [NSHTTPCookieStorage sharedHTTPCookieStorage];
    for (NSHTTPCookie *cookie in [storage cookies]) {
        [storage deleteCookie:cookie];
    }

    NSURL *url = [NSURL URLWithString:@"https://www.baidu.com"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

&#160;

# 4 清除缓存

清除UIWebView的缓存使用了NSURLCache对象，在这里我们使用`removeAllCachedResponses `清楚所有缓存。

```objective-c
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    NSLog(@"清除cookies");
    NSHTTPCookieStorage *storage = [NSHTTPCookieStorage sharedHTTPCookieStorage];
    for (NSHTTPCookie *cookie in [storage cookies]) {
        [storage deleteCookie:cookie];
    }
    
    NSLog(@"清除UIWebView的缓存");
    [[NSURLCache sharedURLCache] removeAllCachedResponses];
    
    NSURL *url = [NSURL URLWithString:@"https://www.baidu.com"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

&#160;

# 5 网页刷新

在UIWebView有下列属性和方法。

- 、@property (nonatomic, readonly, getter=isLoading) BOOL loading、：是否正在刷新界面
- `- (void)reload`：刷新
- `- (void)stopLoading`：停止刷新

在AdvancedVC添加如下代码

```objective-c
#pragma mark 刷新
- (IBAction)reload:(id)sender {
    NSLog(@"%d", self.webView.loading);
    // 正在刷新界面时，停止刷新后重新刷新界面
    if (self.webView.loading) {
        [self.webView stopLoading]; // 停止刷新
    }
    [self.webView reload]; // 刷新界面
}
```

将界面的按钮指向这个方法，运行代码，点击按钮则能看见刷新效果。

&#160;

# 6 网页上一页和下一页

在UIWebView有下列方法：

- `@property (nonatomic, readonly, getter=canGoBack) BOOL canGoBack`：能否去上一页；
- `@property (nonatomic, readonly, getter=canGoForward) BOOL canGoForward`：能否去下一页；
- `- (void)goBack`：去上一页；
- `- (void)goForward`：去下一页。

在AdvancedVC添加如下代码

```objective-c
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
```

将界面上方右边的两个按钮指向这两个方法体，运行项目，则能实现网页前往上一页或下一页。
&#160;

# 7 进度条

## 7.1 QQ进度条分析

打开qq的网页浏览，发现在UINavigationController有一个很炫的进度条，初步分析使用的是UIProgressView控件，这里我们也使用这种控件，只要实现进度条的动画效果即可。如果你想实现更炫的效果，请自行研究UIProgressView。

## 7.2 项目改造

为了实现进度条，需要在界面中加一个UIProgressView控件。

在storyboard拉一个UIProgressView放在view上，并设置相关约束。搭建好后效果如图

![这里写图片描述](http://img.blog.csdn.net/20151104191102858)

然后将控件指向AdvancedVC，并实现UIVWebView的代理，完成后代码如下。

```objective-c
//
//  AdvancedVC.m
//  UIWebView
//
//  Created by yangjun on 15/11/4.
//  Copyright © 2015年 六月. All rights reserved.
//

# import "AdvancedVC.h"

@interface AdvancedVC () <UIWebViewDelegate>
{
    CGFloat _startLoadingCount;  ///< 要加载的链接数
    CGFloat _finishLoadingCount; ///< 已加载的链接数
}

@property (weak, nonatomic) IBOutlet UIWebView *webView; ///< UIWebView
@property (weak, nonatomic) IBOutlet UIProgressView *progressView; ///< 进度条

@end

@implementation AdvancedVC

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    NSLog(@"清除cookies");
    NSHTTPCookieStorage *storage = [NSHTTPCookieStorage sharedHTTPCookieStorage];
    for (NSHTTPCookie *cookie in [storage cookies]) {
        [storage deleteCookie:cookie];
    }
    NSLog(@"清除UIWebView的缓存");
    [[NSURLCache sharedURLCache] removeAllCachedResponses];
    
    NSURL *url = [NSURL URLWithString:@"https://www.baidu.com"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    self.webView.delegate = self;
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
```

## 7.3 进度条实现

运行项目后，我们在浏览器随意操作。发现，有的页面输出信息多，有的页面输出信息少。

对比发现有的页面是请求多个url然后展示，而有的页面只请求一个url页面。

这就给我们实现进度条的机会，我们可以制作一个简单的进度条。由于`webViewDidStartLoad `和`webViewDidFinishLoad `是一一对应的，而且一个页面是并发的请求`webViewDidStartLoad `，然后再一个一个回调`webViewDidFinishLoad `。

通过这里你就发现了，只要完成的请求除以发出的请求就是进度条的百分比。

添加两个属性_startLoadingCount和_finishLoadingCount分别记录需加载和完成的请求，然后相除则完美实现进度条。

这里我只展示核心代码：

```objective-c
@interface AdvancedVC () <UIWebViewDelegate>
{
    CGFloat _startLoadingCount;  ///< 要加载的链接数
    CGFloat _finishLoadingCount; ///< 已加载的链接数
}

@property (weak, nonatomic) IBOutlet UIWebView *webView; ///< UIWebView
@property (weak, nonatomic) IBOutlet UIProgressView *progressView; ///< 进度条

@end

#pragma mark 刷新
- (IBAction)reload:(id)sender {
    NSLog(@"%d", self.webView.loading);
    // 正在刷新界面时，停止刷新后重新刷新界面
    if (self.webView.loading) {
        [self.webView stopLoading]; // 停止刷新
         _startLoadingCount = _finishLoadingCount = 0;
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

#pragma mark - UIWebViewDelegate
#pragma mark 开始加载网页
- (void)webViewDidStartLoad:(UIWebView *)webView {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    [UIApplication sharedApplication].networkActivityIndicatorVisible = YES;
    _startLoadingCount++;
    // 起始设置0.1，让用户感觉在加载
    CGFloat progress = _startLoadingCount == 1 ? 0.1 : self.progressView.progress;
    [self.progressView setProgress:progress animated:YES];
}

#pragma mark 网页加载完成
- (void)webViewDidFinishLoad:(UIWebView *)webView {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    _finishLoadingCount++;
    [self.progressView setProgress:_finishLoadingCount / _startLoadingCount animated:YES];
    // 获取document.readyState状态
    NSString *readyState = [webView stringByEvaluatingJavaScriptFromString:@"document.readyState"];
    BOOL complete = [readyState isEqualToString:@"complete"];
    if (complete && _finishLoadingCount == _startLoadingCount) {
        NSLog(@"加载完成");
        // 还原状态
        _startLoadingCount = _finishLoadingCount = 0;
        [UIApplication sharedApplication].networkActivityIndicatorVisible = NO;
    }
}
```

运行项目，进度条完美运行，是不是感觉很爽，你也会制作很炫的进度条了。但是这里我们制作的进度条是伪进度条，比较粗糙，虽然用户发现不了，但是我们自己还是知道。在UIWebView高级篇我会为大家介绍精度更高的进度条。

![进度条](http://img.blog.csdn.net/20151104191229233)

&#160;

----------

# 其他

## 参考资料

 [UIWebView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIWebView_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-04 | 根据Objective-C中的UIWebView API总结。 |
| 2015-11-04 | 增加关于进度条的实现。 |
| 2015-11-06 | 增加相关文章的链接 |
| 2015-12-11 | 更新相关博文链接 |
| 2015-12-12 | 更新博文名 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog
