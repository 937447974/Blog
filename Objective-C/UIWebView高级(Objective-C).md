[UIWebView基础(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49619847)

[UIWebView进阶(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49638523)

[JavaScriptCore框架](http://blog.csdn.net/y550918116j/article/details/49666443)

[UIWebView高级(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49619847)

----------

看过前两篇博文的朋友，相信对UIWebView有一定的了解。但是有的地方不是很完善，今天是对UIWebView做一定的补充，实现的需求如下：

1. 运用苹果推出的JavaScriptCore实现JS和OC交互；
2. 升级进度条，能够更加精确的捕捉网页加载的进度。

>如果你还不知道JavaScriptCore库，详见我的博文《[JavaScriptCore框架](http://blog.csdn.net/y550918116j/article/details/49666443)》

#JavaScriptCore和UIWebView

在WWDC 2013上，苹果公司推出了JavaScriptCore框架，这是一个基于JavaScript的框架，它完美的以面向对象的方式实现js和oc的交互。

今天我们使用JavaScriptCore为大家介绍更优雅的js和oc交互。

##准备工作

这次创建的项目是借用前两篇博文的组合模式。完成后的界面图如下：

![这里写图片描述](http://img.blog.csdn.net/20151106114855807)

其中有三个控件，刷新按钮，进度条和UIwebView。刷新按钮和UIProgressView主要用于为后面制造高进度的进度条准备。

我们使用新的UIViewController，命名为SeniorVC。我已经为大家搭建了基础代码。

```
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

![这里写图片描述](http://img.blog.csdn.net/20151106115610840)

&#160;

----------

#其他

##参考资料

[UIWebView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIWebView_Class/index.html)

[WWDC 2013: Integrating JavaScript into Native Apps](https://developer.apple.com/videos/play/wwdc2013-615/)

[iOS与JS交互实战篇（ObjC版）](http://mp.weixin.qq.com/s?__biz=MzIzMzA4NjA5Mw==&mid=214063688&idx=1&sn=903258ec2d3ae431b4d9ee55cb59ed89&scene=18#rd)

[iOS7新JavaScriptCore框架入门介绍](http://www.cnblogs.com/ider/p/introduction-to-ios7-javascriptcore-framework.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-4 | 根据Objective-C中的UIWebView API总结。 |
| 2015-11-4 | 增加关于进度条的实现。 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j