[WebKit(展示Web界面)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(展示Web界面).md)

[WebKit(WKScriptMessageHandler)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKScriptMessageHandler).md)

[WebKit(WKUIDelegate)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKUIDelegate).md)

[WebKit(WKNavigationDelegate)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKNavigationDelegate).md)

[WebKit(刷新)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(刷新).md)

[WebKit(导航)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(导航).md)

[WebKit(浏览记录)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(浏览记录).md)

[WebKit(进度条)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(进度条).md)

------

上一篇博文《[WebKit(展示Web界面)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(展示Web界面).md)》讲解了显示Web页面，这一篇博文将讲解使用WKScriptMessageHandler完成JS交互。

在WKWebView中OC和JS交互也非常简单，WebKit库中有个代理WKScriptMessageHandler就是专门来做交互的。

#1 WKScriptMessageHandler

##1.1 WKScriptMessageHandler协议

WKScriptMessageHandler其实就是一个遵循的协议，它能让网页通过JS把消息发送给OC。其中协议方法。

```objc
- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message;
```

从协议中我们可以看出这里使用了两个类WKUserContentController和WKScriptMessage。WKUserContentController可以理解为调度器，WKScriptMessage则是携带的数据。

##1.2 WKUserContentController

WKUserContentController有两个核心方法，也是它的核心功能。

1. `- (void)addUserScript:(WKUserScript *)userScript;`: js注入，即向网页中注入我们的js方法，这是一个非常强大的功能，开发中要慎用。
2. `- (void)addScriptMessageHandler:(id <WKScriptMessageHandler>)scriptMessageHandler name:(NSString *)name;`：添加供js调用oc的桥梁。这里的name对应WKScriptMessage中的name，多数情况我们认为它就是方法名。

##1.3 WKScriptMessage

WKScriptMessage就是js通知oc的数据。其中有两个核心属性用的很多。

1. `@property (nonatomic, readonly, copy) NSString *name;` ：对应`- (void)addScriptMessageHandler:(id <WKScriptMessageHandler>)scriptMessageHandler name:(NSString *)name;`添加的name
2. `@property (nonatomic, readonly, copy) id body;`：携带的核心数据。

js调用时只需

```objc
window.webkit.messageHandlers.<name>.postMessage(<messageBody>)
```

这里的name就是我们添加的name，是不是感觉很爽，就是这么简单，下面我们就来具体实现。

#2 JS调用OC

##2.1 配置WKUserContentController

要想使用WKUserContentController为web页面添加桥梁，只需配置到WKWebViewConfiguration即可。

下面改造webView方法。


```objc
#pragma mark - get方法
- (WKWebView *)webView {
    if (_webView == nil) {
        // js配置
        WKUserContentController *userContentController = [[WKUserContentController alloc] init];
        [userContentController addScriptMessageHandler:self name:@"jsCallOC"];

        // WKWebView的配置
        WKWebViewConfiguration *configuration = [[WKWebViewConfiguration alloc] init];
        configuration.userContentController = userContentController;
        
        // 显示WKWebView
        _webView = [[WKWebView alloc] initWithFrame:self.view.frame configuration:configuration];
        _webView.UIDelegate = self; // 设置WKUIDelegate代理
        [self.view addSubview:_webView];
    }
    return _webView;
}
```

##2.2 实现WKScriptMessageHandler

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

##2.3 改造index.html页面

修改index.html的

```js
// 点击确定按钮
function onClickButton() {
    // 复杂数据
    var list = [1,2,3];
    var dict = {"name":"阳君", "qq":"937447974", "data":input.value, "list":list};
    alert(dict);
    // JS通知WKWebView
    window.webkit.messageHandlers.jsCallOC.postMessage(dict);
}
```

这里我们为了测试效果传入了一个复杂的字典数据，而且字典中还有数组。input.value代表用户输入的数据。

这里使用了`window.webkit.messageHandlers.jsCallOC.postMessage(dict);`通知oc，jsCallOC这个属性就是前面我们通过WKUserContentController注入的。

##2.4 测试交互

我们在viewDidLoad完成测试，加载index.html页面。

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
//    [self loadWebView]; // 加载测试
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"index" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url];
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

运行项目后，在页面的输入框中输入相应信息，点击确定按钮。即可在xcode中看见相关打印信息。

#3 OC调用JS

##3.1 OC通知JS

oc调用js就特别简单了，只需WKWebView调用`- (void)evaluateJavaScript:(NSString *)javaScriptString completionHandler:(void (^ __nullable)(__nullable id, NSError * __nullable error))completionHandler;`方法即可。

在上面的打印信息中，我们会发现我们还没有实现jsCallOC：方法，接下来我们在jsCallOC:方法中实现通知JS，并将用户输入的信息发送给JS。

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

##3.2 JS响应

OC将通知发送给JS后，JS要响应这个ocCallJS方法。我们在index.html中的onClickButton()方法下添加ocCallJS方法。

```js
// WKWebView调用JS
function ocCallJS(params) {
    show.innerHTML = params;
}
```

运行项目，在输入框输入信息后，点击确定按钮，会发现输入的信息在输入框下面显示出来。如图所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015120104.jpg)



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