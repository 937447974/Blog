[WebKit(展示Web界面)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(展示Web界面).md)

[WebKit(WKScriptMessageHandler)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(WKScriptMessageHandler).md)

[WebKit(WKUIDelegate)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(WKUIDelegate).md)

[WebKit(WKNavigationDelegate)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(WKNavigationDelegate).md)

[WebKit(刷新)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(刷新).md)

[WebKit(导航)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(导航).md)

[WebKit(浏览记录)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(浏览记录).md)

[WebKit(进度条)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(进度条).md)

------

上一篇博文《[WebKit(展示Web界面)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(展示Web界面).md)》讲解了显示Web页面，这一篇博文将讲解使用WKScriptMessageHandler完成JS交互。

在WKWebView中OC和JS交互也非常简单，WebKit库中有个代理WKScriptMessageHandler就是专门来做交互的。

# 1 WKScriptMessageHandler

## 1.1 WKScriptMessageHandler协议

WKScriptMessageHandler其实就是一个遵循的协议，它能让网页通过JS把消息发送给OC。其中协议方法。

```objc
/*! @abstract Invoked when a script message is received from a webpage.
 @param userContentController The user content controller invoking the
 delegate method.
 @param message The script message received.
 */
- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message;
```

从协议中我们可以看出这里使用了两个类WKUserContentController和WKScriptMessage。WKUserContentController可以理解为调度器，WKScriptMessage则是携带的数据。

## 1.2 WKUserContentController

WKUserContentController有两个核心方法，也是它的核心功能。

1. `- (void)addUserScript:(WKUserScript *)userScript;`: js注入，即向网页中注入我们的js方法，这是一个非常强大的功能，开发中要慎用。
2. `- (void)addScriptMessageHandler:(id <WKScriptMessageHandler>)scriptMessageHandler name:(NSString *)name;`：添加供js调用oc的桥梁。这里的name对应WKScriptMessage中的name，多数情况下我们认为它就是方法名。

## 1.3 WKScriptMessage

WKScriptMessage就是js通知oc的数据。其中有两个核心属性用的很多。

1. `@property (nonatomic, readonly, copy) NSString *name;` ：对应`- (void)addScriptMessageHandler:(id <WKScriptMessageHandler>)scriptMessageHandler name:(NSString *)name;`添加的name。
2. `@property (nonatomic, readonly, copy) id body;`：携带的核心数据。

js调用时只需

```objc
window.webkit.messageHandlers.<name>.postMessage(<messageBody>)
```

这里的name就是我们添加的name，是不是感觉很爽，就是这么简单，下面我们就来具体实现。

# 2 JS调用OC

## 2.1 配置WKUserContentController

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

## 2.2 实现WKScriptMessageHandler

在当前页面引入WKScriptMessageHandler，并实现WKScriptMessageHandler协议即可。

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

## 2.3 改造index.html页面

修改index.html的onClickButton()方法。

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

## 2.4 测试交互

我们在viewDidLoad使用index.html页面完成测试。

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

# 3 OC调用JS

## 3.1 OC通知JS

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

## 3.2 JS响应

OC将通知发送给JS后，JS要响应这个ocCallJS方法。我们在index.html中的onClickButton()方法下添加ocCallJS方法。

```js
// WKWebView调用JS
function ocCallJS(params) {
    show.innerHTML = params;
}
```

运行项目，在输入框输入信息后，点击确定按钮，会发现输入的信息在输入框下面显示出来。如图所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015120104.jpg)

并在控制台看见如下打印信息。

```
2015-12-01 20:16:08.567 WebKit[5034:1216608] 方法名:jsCallOC
2015-12-01 20:16:08.568 WebKit[5034:1216608] 参数:{
    data = "\U9633\U541b";
    list =     (
        1,
        2,
        3
    );
    name = "\U9633\U541b";
    qq = 937447974;
}
```

# 4 WKUserScript JS注入

## 4.1 WKUserScript核心方法

在WebKit框架中，我们还可以预先添加JS方法，供其他人员调用。WKUserScript就是帮助我们完成JS注入的类，它能帮助我们在页面填充前或js填充完成后调用。核心方法。

```objc
/*! @abstract Returns an initialized user script that can be added to a @link WKUserContentController @/link.
 @param source The script source.
 @param injectionTime When the script should be injected.
 @param forMainFrameOnly Whether the script should be injected into all frames or just the main frame.
 */
- (instancetype)initWithSource:(NSString *)source injectionTime:(WKUserScriptInjectionTime)injectionTime forMainFrameOnly:(BOOL)forMainFrameOnly;
```

## 4.2 WKUserScriptInjectionTime枚举

在WKUserScriptInjectionTime枚举中有两个状态。

1. WKUserScriptInjectionTimeAtDocumentStart：js加载前执行。
2. WKUserScriptInjectionTimeAtDocumentEnd：js加载后执行。

## 4.3 js注入

WKUserScript的运行需依托WKUserContentController，接下来我们就为WKWebView注入一个js执行完毕后执行的alert方法。

改造`- (WKWebView *)webView`方法。

```objc
#pragma mark - get方法
- (WKWebView *)webView {
    if (_webView == nil) {
        // js配置
        WKUserContentController *userContentController = [[WKUserContentController alloc] init];
        [userContentController addScriptMessageHandler:self name:@"jsCallOC"];
        // js注入，注入一个alert方法，页面加载完毕弹出一个对话框。
        NSString *javaScriptSource = @"alert(\"WKUserScript注入js\");";
        WKUserScript *userScript = [[WKUserScript alloc] initWithSource:javaScriptSource injectionTime:WKUserScriptInjectionTimeAtDocumentEnd forMainFrameOnly:YES];// forMainFrameOnly:NO(全局窗口)，yes（只限主窗口）
        [userContentController addUserScript:userScript];
        
        // WKWebView的配置
        WKWebViewConfiguration *configuration = [[WKWebViewConfiguration alloc] init];
        configuration.userContentController = userContentController;
        
        // 显示WKWebView
        _webView = [[WKWebView alloc] initWithFrame:self.view.frame configuration:configuration];
        [self.view addSubview:_webView];
    }
    return _webView;
}
```

&#160;

----------

# 其他

## 源代码

[Objective-C](https://github.com/937447974/Objective-C)

## 参考资料

[WebKit Framework Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/WebKit/ObjC_classic/index.html#//apple_ref/doc/uid/TP30000745)

[WKWebView的新特性与使用](http://www.brighttj.com/ios/ios-wkwebview-new-features-and-use.html)

[WKWeb​View](http://nshipster.com/wkwebkit/?utm_campaign=iOS_Dev_Weekly_Issue_161&utm_medium=email&utm_source=iOS%2BDev%2BWeekly)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-30 | 博文完成 |
| 2015-12-12 | 更改链接 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog