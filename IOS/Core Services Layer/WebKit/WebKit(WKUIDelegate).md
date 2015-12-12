[WebKit(展示Web界面)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(展示Web界面).md)

[WebKit(WKScriptMessageHandler)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(WKScriptMessageHandler).md)

[WebKit(WKUIDelegate)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(WKUIDelegate).md)

[WebKit(WKNavigationDelegate)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(WKNavigationDelegate).md)

[WebKit(刷新)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(刷新).md)

[WebKit(导航)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(导航).md)

[WebKit(浏览记录)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(浏览记录).md)

[WebKit(进度条)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(进度条).md)

------

在上一篇《[WebKit(WKScriptMessageHandler)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKScriptMessageHandler).md)》博文中，最后我们运行项目后，发现并没有在app上看见alert弹出框，其实这不是一个bug。而是苹果WebKit库的又一升级。

WebKit考虑web页面的各种弹出框样式，如alert弹出框、Confirm选择框和TextInput输入框。WebKit为了让我们开发人员可以根据自己APP的风格设计各种不同的样式。对这些弹出框进行了封装。

这一列的操作都是基于WKUIDelegate协议实现的，本篇博文将为大家讲解WKUIDelegate协议。

#1 改造index.html页面

要实现这三个弹出框，我们先改造我们前面用到的index.html页面，使其支持alert弹出框、Confirm选择框和TextInput输入框。

下面是所有源码，复制替换即可。

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

运行项目，看见如下效果图。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015113003.jpg)

当然此时你点击这三个按钮是没有任何效果的，我们还没有实现WKUIDelegate协议。

#2 WKUIDelegate协议

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

#3 实现WKUIDelegate

下面就来实现这几个协议，让我们的项目完整起来。

##3.1 引入WKUIDelegate

在YJBaseVC.m上添加WKUIDelegate实现。

```objc
@interface YJBaseVC () <WKScriptMessageHandler, WKUIDelegate>
```

##3.2 设置WKUIDelegate代理

修改`- (WKWebView *)webView`方法。

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
        _webView.UIDelegate = self; // 设置WKUIDelegate代理
        [self.view addSubview:_webView];
    }
    return _webView;
}
```

##3.3 实现WKUIDelegate协议

在YJBaseVC.m添加如下方法。

```objc
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

这里使用UIAlertController为大家展示这种效果，运行项目点击页面上的三个按钮即可看到相应的效果图。

到这里关于WebKit基础部分就讲解完毕了，已支持市面上大多数的需求。后面会讲解关于WebKit的高级应用，模拟浏览器操作。
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
| 2015-12-12 | 更改链接 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog