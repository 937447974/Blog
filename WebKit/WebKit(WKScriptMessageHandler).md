在WWDC2014中，苹果推出了最新的iOS8系统，其中也伴随着很多控件的更新与升级。其中全新的WebKit库让人很是兴奋。本文也将讲解使用WebKit中更新的WKWebView控件加载网页、以及和javascript交互。它很好的解决了UIWebView存在的内存、加载速度等诸多问题。

> WebKit的核心就是WKWebView控件。

#1 准备工作

##1.1 Html页面

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

##1.2 搭建项目

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

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015113003.jpg)

#3 JS交互

在WKWebView中OC和JS交互也非常简单，WebKit库中有个代理WKScriptMessageHandler就是专门来做交互的。

##3.1 WKScriptMessageHandler

WKScriptMessageHandler其实就是一个遵循的协议，它能让网页通过JS把消息发送给OC。其中协议方法。

```objc
- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message;
```

从协议中我们可以看出这里使用了两个类WKUserContentController和WKScriptMessage。WKUserContentController可以理解为调度器，WKScriptMessage则是携带的数据。

##3.2 WKUserContentController

WKUserContentController有两个核心方法，也是它的核心功能。

1. `- (void)addUserScript:(WKUserScript *)userScript;`: js注入，即向网页中注入我们的js方法，这是一个非常强大的功能，开发中要慎用。
2. `- (void)addScriptMessageHandler:(id <WKScriptMessageHandler>)scriptMessageHandler name:(NSString *)name;`：添加供js调用oc的桥梁。这里的name对应WKScriptMessage中的name，多数情况我们认为它就是方法名。

##3.3 WKScriptMessage

WKScriptMessage就是js通知oc的数据。其中有两个核心属性用的很多。

1. `@property (nonatomic, readonly, copy) NSString *name;` ：对应`- (void)addScriptMessageHandler:(id <WKScriptMessageHandler>)scriptMessageHandler name:(NSString *)name;`添加的name
2. `@property (nonatomic, readonly, copy) id body;`：携带的核心数据。

js调用时只需

```objc
window.webkit.messageHandlers.<name>.postMessage(<messageBody>)
```

这里的name就是我们添加的name，是不是感觉很爽，就是这么简单，下面我们就来具体实现。

##3.4 JS调用OC

###3.4.1 配置WKUserContentController

要想使用WKUserContentController为web页面添加桥梁，只需配置到WKWebViewConfiguration即可。

下面改造viewDidLoad方法。


```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    
    // js配置
    WKUserContentController *userContentController = [[WKUserContentController alloc] init];
    [userContentController addScriptMessageHandler:self name:@"jsCallOC"];

    // WKWebView的配置
    WKWebViewConfiguration *configuration = [[WKWebViewConfiguration alloc] init];
    configuration.userContentController = userContentController;
    
    // 显示网页
    self.webView = [[WKWebView alloc] initWithFrame:self.view.frame  configuration:configuration];
    [self.view addSubview:self.webView];

    // 找到index.html的路径
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"index" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

###3.4.2 实现WKScriptMessageHandler

在当前页面引入WKScriptMessageHandler并实现即可。

```objc
@interface YJBaseVC () <WKScriptMessageHandler>

#pragma mark - WKScriptMessageHandler
- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message {
    NSLog(@"方法名:%@", message.name);
    NSLog(@"参数:%@", message.body);
    // 方法名
    NSString *methods = [NSString stringWithFormat:@"%@:", message.name];
    SEL selector = NSSelectorFromString(methods);
    // 调用方法
    if ([self respondsToSelector:selector]) {
        [self performSelector:selector withObject:message.body];
    } else {
        NSLog(@"未实行方法：%@", methods);
    }
}
```

这样就实现了js调用OC了，你可以运行项目点击确定按钮，在控制台查看输出。当然你在控制台会看见“未实行方法：jsCallOC：”，我们还没有实现jsCallOC：这个方法。

##3.5 OC调用JS

oc调用js就特别简单了，只需WKWebView调用`- (void)evaluateJavaScript:(NSString *)javaScriptString completionHandler:(void (^ __nullable)(__nullable id, NSError * __nullable error))completionHandler;`方法即可。

我们在jsCallOC：中把用户输入的数据回调给html页面。

```objc
#pragma mark js调OC
- (void)jsCallOC:(id)body {
    if ([body isKindOfClass:[NSDictionary class]]) {
        NSDictionary *dict = (NSDictionary *)body;
        // oc调用js代码
        NSString *jsStr = [NSString stringWithFormat:@"ocCallJS('%@')", [dict objectForKey:@"data"]];
        [self.webView evaluateJavaScript:jsStr completionHandler:^(id _Nullable data, NSError * _Nullable error) {
            if (error) {
                NSLog(@"错误:%@", error.localizedDescription);
            }
        }];
    }
}
```

##3.6 WKUserScript JS注入

WKUserScript就是帮助我们完成JS注入的类，它能帮助我们在页面填充前或js填充完成后调用。核心方法。

```objc
/*! @abstract Returns an initialized user script that can be added to a @link WKUserContentController @/link.
 @param source The script source.
 @param injectionTime When the script should be injected.
 @param forMainFrameOnly Whether the script should be injected into all frames or just the main frame.
 */
- (instancetype)initWithSource:(NSString *)source injectionTime:(WKUserScriptInjectionTime)injectionTime forMainFrameOnly:(BOOL)forMainFrameOnly;
```

WKUserScript的运行需依托WKUserContentController。

再次改造viewDidLoad方法。

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    
    // js配置
    WKUserContentController *userContentController = [[WKUserContentController alloc] init];
    [userContentController addScriptMessageHandler:self name:@"jsCallOC"];
    // js注入，注入一个ale方法，会显示一个弹出框。
    NSString *javaScriptSource = @"function ale() { alert(\"WKUserScript注入js\");}";
    WKUserScript *userScript = [[WKUserScript alloc] initWithSource:javaScriptSource injectionTime:WKUserScriptInjectionTimeAtDocumentEnd forMainFrameOnly:NO];// forMainFrameOnly:NO(全局窗口)，yes（只限主窗口）
    [userContentController addUserScript:userScript];

    // WKWebView的配置
    WKWebViewConfiguration *configuration = [[WKWebViewConfiguration alloc] init];
    configuration.userContentController = userContentController;
    
    // 显示网页
    self.webView = [[WKWebView alloc] initWithFrame:self.view.frame  configuration:configuration];
    [self.view addSubview:self.webView];
    
    // 找到index.html的路径
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"index" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

这里改写了html页面的ale()方法，并在js执行完毕后注入，这样会覆盖html中的ale()方法。

#4 对话框

运行项目你会发现点击警告框、选择框和输入框这三个按钮都没有任何作用，这是因为WebKit库想让我们自己去
实现这些方法，给用户更好的体验。

##4.1 WKUIDelegate

WKUIDelegate就是为了UI元素存在的，其中有几个协议。

```objc
// 新建WKWebView
- (nullable WKWebView *)webView:(WKWebView *)webView createWebViewWithConfiguration:(WKWebViewConfiguration *)configuration forNavigationAction:(WKNavigationAction *)navigationAction windowFeatures:(WKWindowFeatures *)windowFeatures;

// 关闭WKWebView
- (void)webViewDidClose:(WKWebView *)webView NS_AVAILABLE(10_11, 9_0);

// 对应js的Alert方法
/**
 *  web界面中有弹出警告框时调用
 *
 *  @param webView           实现该代理的webview
 *  @param message           警告框中的内容
 *  @param frame             主窗口
 *  @param completionHandler 警告框消失调用
 */
- (void)webView:(WKWebView *)webView runJavaScriptAlertPanelWithMessage:(NSString *)message initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(void))completionHandler;

// 对应js的confirm方法
- (void)webView:(WKWebView *)webView runJavaScriptConfirmPanelWithMessage:(NSString *)message initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(BOOL result))completionHandler;

// 对应js的prompt方法
- (void)webView:(WKWebView *)webView runJavaScriptTextInputPanelWithPrompt:(NSString *)prompt defaultText:(nullable NSString *)defaultText initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(NSString * __nullable result))completionHandler;
```

##4.2 实现WKUIDelegate

下面就来实现这几个协议，让我们的项目完整起来。

```objc
@interface YJBaseVC () <WKScriptMessageHandler, WKUIDelegate>


// 设置WKUIDelegate代理
self.webView.UIDelegate = self;


#pragma mark - WKUIDelegate
#pragma mark 新建webView
- (WKWebView *)webView:(WKWebView *)webView createWebViewWithConfiguration:(WKWebViewConfiguration *)configuration forNavigationAction:(WKNavigationAction *)navigationAction windowFeatures:(WKWindowFeatures *)windowFeatures {
    NSLog(@"%s",__FUNCTION__);
    return webView;
}

#pragma mark 关闭webView
- (void)webViewDidClose:(WKWebView *)webView {
    NSLog(@"%s",__FUNCTION__);
}

#pragma mark alert弹出框
- (void)webView:(WKWebView *)webView runJavaScriptAlertPanelWithMessage:(NSString *)message initiatedByFrame:(WKFrameInfo *)frame completionHandler:(void (^)(void))completionHandler {
    NSLog(@"%s",__FUNCTION__);
    // 确定按钮
    UIAlertAction *alertAction = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
        completionHandler();
    }];
    // alert弹出框
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:message message:nil preferredStyle:UIAlertControllerStyleAlert];
    [alertController addAction:alertAction];
    [self presentViewController:alertController animated:YES completion:nil];
}

#pragma mark Confirm选择框
- (void)webView:(WKWebView *)webView runJavaScriptConfirmPanelWithMessage:(nonnull NSString *)message initiatedByFrame:(nonnull WKFrameInfo *)frame completionHandler:(nonnull void (^)(BOOL))completionHandler {
    NSLog(@"%s",__FUNCTION__);
    // 按钮
    UIAlertAction *alertActionCancel = [UIAlertAction actionWithTitle:@"Cancel" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
        // 返回用户选择的信息
        completionHandler(NO);
    }];
    UIAlertAction *alertActionOK = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
        completionHandler(YES);
    }];
    // alert弹出框
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:message message:nil preferredStyle:UIAlertControllerStyleAlert];
    [alertController addAction:alertActionCancel];
    [alertController addAction:alertActionOK];
    [self presentViewController:alertController animated:YES completion:nil];
}

#pragma mark TextInput输入框
- (void)webView:(WKWebView *)webView runJavaScriptTextInputPanelWithPrompt:(nonnull NSString *)prompt defaultText:(nullable NSString *)defaultText initiatedByFrame:(nonnull WKFrameInfo *)frame completionHandler:(nonnull void (^)(NSString * _Nullable))completionHandler {
    NSLog(@"%s",__FUNCTION__);
    // alert弹出框
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:prompt message:nil preferredStyle:UIAlertControllerStyleAlert];
    // 输入框
    [alertController addTextFieldWithConfigurationHandler:^(UITextField * _Nonnull textField) {
        textField.placeholder = defaultText;
    }];
    // 确定按钮
    [alertController addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
        // 返回用户输入的信息
        UITextField *textField = alertController.textFields.firstObject;
        completionHandler(textField.text);
    }]];
    // 显示
    [self presentViewController:alertController animated:YES completion:nil];
}
```

#5 小结

在本篇博文讲解了WebKit的相关基础知识，针对大多数的项目已完全够用。其中包含了js和oc交互、js注入到网页和自定义对话框。
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