在WWDC2014中，苹果推出了最新的iOS8系统，其中也伴随着很多控件的更新与升级。其中全新的WebKit库让人很是兴奋。本文也将讲解使用WebKit中更新的WKWebView控件加载网页、以及和javascript交互。它很好的解决了UIWebView存在的内存、加载速度等诸多问题。

> WebKit的核心就是WKWebView控件。

#1 准备工作

#1.1 Html页面

为了不借助服务器，以及相关其他网页，本次项目是在本地搭建一个网页，下面是网页的全部代码。

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
        // 复杂数据
        var list = [1,2,3];
        var dict = {"name":"阳君", "qq":"937447974", "data":input.value, "list":list};
        alert(dict);
        // JS通知WKWebView
        window.webkit.messageHandlers.jsCallOC.postMessage(dict);
    }

    // WKWebView调用JS
    function ocCallJS(params) {
        show.innerHTML = params;
    }

    // alert对话框
    function ale() {
        alert("这是一个警告对话框！");
    }

    // confirm选择框
    function firm() {
        if(confirm("去百度看看？")) {
            alert("你选择了去！");
        } else {
            alert("你选择了不去！");
        }
    }

    // prompt输入框
    function prom() {
        var result = prompt("演示一个带输入的对话框", "这里输入你的信息");
        if(result) {
            alert("谢谢使用，你输入的是：" + result)
        }
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
            <br><br>
            <input type="submit" value="警告框" onclick="ale()" />
            <input type="submit" value="选择框" style="margin-left:20px;" onclick="firm()" />
            <input type="submit" value="输入框" style="margin-left:20px;" onclick="prom()" />
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

在这个界面已经初始化了后续会调用的js代码，以及UI界面。在UI界面有1个输入框、1个显示框和4个按钮，会在后期用到。这里不做详细介绍。

你在浏览器运行会看见如下效果图。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015113002.png)

#1.2 搭建项目

这次启用了和讲解UIWebView相类似的项目。完整项目各位可自行搭建，我这里搭建的项目如下。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015113001.png)

在基础篇使用了类YJBaseVC，YJSeniorVC会在高级篇使用。

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

#2 网页展示

显示网页需要使用WKWebView，这里不能使用在xib和storyboard拉控件的方式拉取，因为还没有这样的控件，只能使用代码的方式创建WKWebView。

##2.1 增加WebKit库

WKWebView的运行都要基于WKWebView库，故我们添加WKWebView库。

```objc
#import <WebKit/WebKit.h>
```

##2.2 显示网页

WKWebView有一个核心配置器WKWebViewConfiguration，你可以理解它是WKWebView的中央管理器。这里设置一个空的WKWebViewConfiguration，后续会做补充。

在viewDidLoad方法中添加如下代码。

```objc
- (void)viewDidLoad {
    [super viewDidLoad];

    // WKWebView的配置
    WKWebViewConfiguration *configuration = [[WKWebViewConfiguration alloc] init];
    
    // 显示网页
    self.webView = [[WKWebView alloc] initWithFrame:self.view.frame  configuration:configuration];
    [self.view addSubview:self.webView];
    
    // 找到index.html的路径
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"index" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

这里我们使用了本地加载html页面的方式，你还可以使用其他方法加载网页页面，在WKWebView中显示网页有如下方式。加载网页有如下4种。

```objc
- (nullable WKNavigation *)loadRequest:(NSURLRequest *)request;

- (nullable WKNavigation *)loadFileURL:(NSURL *)URL allowingReadAccessToURL:(NSURL *)readAccessURL NS_AVAILABLE(10_11, 9_0);

- (nullable WKNavigation *)loadHTMLString:(NSString *)string baseURL:(nullable NSURL *)baseURL;

- (nullable WKNavigation *)loadData:(NSData *)data MIMEType:(NSString *)MIMEType characterEncodingName:(NSString *)characterEncodingName baseURL:(NSURL *)baseURL
```

见名知意，这里就不详细说明了。运行项目会在手机看到如下效果图。








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