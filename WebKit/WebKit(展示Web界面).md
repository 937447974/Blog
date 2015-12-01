[WebKit(展示Web界面)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(展示Web界面).md)

[WebKit(WKScriptMessageHandler)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKScriptMessageHandler).md)

[WebKit(WKUIDelegate)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKUIDelegate).md)

[WebKit(WKNavigationDelegate)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKNavigationDelegate).md)

[WebKit(刷新)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(刷新).md)

[WebKit(导航)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(导航).md)

[WebKit(浏览记录)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(浏览记录).md)

[WebKit(进度条)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(进度条).md)

------

在WWDC2014中，苹果推出了最新的iOS8系统，其中也伴随着很多控件的更新与升级。其中全新的WebKit库让人很是兴奋。本人将用一系列的博文来为大家讲解WebKit的相关应用。在本篇博文将为大家讲解使用WKWebView怎么加载本地和网络Web页面。

> WebKit的核心就是WKWebView控件。

#1 项目

#1.1 搭建项目

这次启用了和讲解UIWebView相类似的项目。完整项目各位可自行搭建，我这里搭建的项目图如下。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015113001.png)

在这里使用了类YJBaseVC，后续会使用YJSeniorVC。

```objc
//
//  YJBaseVC.m
//  WebKit
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/29.
//  Copyright © 2015年 阳君. All rights reserved.
//

#import "YJBaseVC.h"

@interface YJBaseVC ()

@property (nonatomic, strong) WKWebView *webView; ///< WKWebView

@end

@implementation YJBaseVC

- (void)viewDidLoad {
    [super viewDidLoad];
    
}

@end
```

这里只有一个全局属性webView，它指向一个强引用的WKWebView类。

#1.2 初始化WKWebView

##1.2.1 增加WebKit库

WKWebView的运行都要基于WKWebView库，故我们添加WKWebView库。

```objc
#import <WebKit/WebKit.h>
```

##1.2.2 懒加载WKWebView

在这里我们使用懒加载的方式加载WKWebView，即使用的时候才添加到View中。

WKWebView有一个核心配置器WKWebViewConfiguration，你可以理解它是WKWebView的中央管理器。这里设置一个空的WKWebViewConfiguration，后续会做补充。

在YJBaseVC.m添加方法。

```objc
#pragma mark - get方法
- (WKWebView *)webView {
    if (_webView == nil) {
        // WKWebView的配置
        WKWebViewConfiguration *configuration = [[WKWebViewConfiguration alloc] init];
        // 显示WKWebView
        _webView = [[WKWebView alloc] initWithFrame:self.view.frame configuration:configuration];
        [self.view addSubview:_webView];
    }
    return _webView;
}
```

#2 显示本地Html页面

##2.1 搭建本地Html页面

下面就是我为大家搭建的网页源码。

```html
<!doctype html>
<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8">
<meta name="viewport" content="width=device-width, maximum-scale=2, minimum-scale=1, user-scalable=no">
<title>首页</title>
<style type="text/css">
    *{
        margin:0;
        padding: 0;
    }
    body,html{
        height: 100%;
        width: 100%;
    }
    section{
        height: 100%;
        width: 100%;
        text-align:center;
    }
    .div{
        float: left;
        position: relative;
        left: 50%;
        top: 30%;
        margin-left: -113px;
        margin-top: -20px;
    }
</style>
<script>
    // $解析器
    function $ (ele) {
        return document.querySelector(ele);
    };

    // 点击确定按钮
    function onClickButton() {
        alert(input.value);
    }
</script>
</head>

<body >
    <section>
        <div class="div">
            <input type="text" id="input" style="width:150px;line-height:30px">
            <a style="margin-left:10px;width:50px;line-height:30px;display:inline-block;background-color:blue;color:#fff;text-align:center;" id="button" >确定</a>
            <br>
            <span id="show" style="display:inline-block;width:100%;text-align:left;font-size:18px;font-family:'微软雅黑';color:#000;margin-top:20px;" ></span>
            </div>
    </section>
    <script type="text/javascript">
        // 界面渲染完毕执行
        var input = $('#input'),
        button = $('#button'),
        show = $('#show');
        // 按钮监听
        button.addEventListener('click', onClickButton);
    </script>
</body>
</html>
```

你可以在项目中新建一个文件，将代码复制进去，并设文件名为index.html。在浏览器运行会看见如下效果图。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015120101.png)

##2.2 加载Html页面

在WKWebView加载页面时常用方法`- (nullable WKNavigation *)loadRequest:(NSURLRequest *)request;`。还有其他几种加载方法，可自行研究，这里不在描述。

添加如下方法。

```objc
#pragma mark webView 加载测试
- (void)loadWebView {
    // 本地html页面
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"index" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

改写viewDidLoad方法。

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    [self loadWebView]; // 加载测试
}
```

运行即可看到效果。

#2 网络页面展示

##2.1 加载百度首页

在这里我们使用百度首页作为我们要显示的页面。

改写loadWebView方法。

```objc
#pragma mark webView 加载测试
- (void)loadWebView {
    // 本地html页面
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"index" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面
    
    // 网页路径
    url = [NSURL URLWithString:@"https://www.baidu.com"];
    urlRequest = [NSURLRequest requestWithURL:url];
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

运行项目发现百度首页无法显示。

##2.2 解决网络页面无法显示问题

由于IOS9的安全机制更高，苹果不再允许http连接和没有ssl验证的https运行。

但我们可以人为的解决这种问题，只需在Info.plist文件添加如下代码。

```objc
<key>NSAppTransportSecurity</key>
 <dict>
 <key>NSAllowsArbitraryLoads</key>
 <true/>
 </dict>
```

运行项目看见百度首先显示了。


![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015113003.jpg)


&#160;

----------

#其他

##源代码

[Objective-C](https://github.com/937447974/Objective-C)

##参考资料

[WebKit Framework Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/WebKit/ObjC_classic/index.html#//apple_ref/doc/uid/TP30000745)

[WKWebView的新特性与使用](http://www.brighttj.com/ios/ios-wkwebview-new-features-and-use.html)

[WKWeb​View](http://nshipster.com/wkwebkit/?utm_campaign=iOS_Dev_Weekly_Issue_161&utm_medium=email&utm_source=iOS%2BDev%2BWeekly)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-30 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog