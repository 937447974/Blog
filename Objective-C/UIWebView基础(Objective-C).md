当你在项目中想嵌入网页时，可以使用UIWebView类嵌入Web内容。你只需要创建一个UIWebView对象，并将它附加到一个view窗口。你还可以使用这个类来执行页面历史的前进或后退。

本篇博文主要介绍关于UIWebView的基础，包括：加载网页、实现代理以及JS和OC的互相调用。
&#160;

#准备工作
##Html页面

我已经为大家创建了html页面的源代码，只需要复制到记事本，并将文件名改为index.html。

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
        show.innerHTML = input.value;
    }
</script>
</head>

<body >
    <section>
        <div class="div">
            <input type="text" id="input" style="width:150px;line-height:30px">
            <a style="margin-left:10px;width:50px;line-height:30px;display:inline-block;background-color:blue;color:#fff;text-align:center;" id="button" >按钮</a>
            <br>
            <span id="show" style="display:inline-block;width:100%;text-align:left;font-size:18px;font-family:'微软雅黑';color:#000;background-color:pink;margin-top:20px;" ></span>
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

这个页面有输入框、按钮和做显示用的，其简单功能是在输入框输入数据，点击按钮后显示到显示区域。

在浏览器运行可以看到如下效果。

![浏览器](http://img.blog.csdn.net/20151103175907757)

##搭建项目

这个地方就不详细描述了，创建一个简单项目->拉一个UIWebView到界面->UIWebView指向UIViewController的属性名。

搭建后的核心部分如下，这里我使用的VC名为BaseVC。

```Oc
//
//  BaseVC.m
//  WebView
//
//  Created by yangjun on 15/11/3.
//  Copyright © 2015年 六月. All rights reserved.
//

#import "BaseVC.h"

@interface BaseVC () <UIWebViewDelegate>

@property (weak, nonatomic) IBOutlet UIWebView *webView; ///< UIWebView

@end

@implementation BaseVC

- (void)viewDidLoad {
    [super viewDidLoad];
}

@end

```

&#160;

#显示Web页面

将我们创建好的index.html拉到项目中，至于位置就随你高兴了。

然后我们改造BaseVC的viewDidLoad方法。

```oc
- (void)viewDidLoad {
    [super viewDidLoad];
    // 找到index.html的路径
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"index" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

由于我们的html页面在项目里面，我们可以直接使用[NSBundle mainBundle]去寻找。如果你使用的是网咯连接“www.baidu.com”,你可以这样获得NSURL。

```oc
url = [NSURL URLWithString:@"www.baidu.com"];
```

然后运行项目，就可以看到和浏览器一样的效果。

![app](http://img.blog.csdn.net/20151103175852377)

&#160;

#代理UIWebViewDelegate

UIWebView也有代理，如果你不懂什么是代理模式，查阅我的博文《[ 23设计模式之代理模式(Proxy)](http://blog.csdn.net/y550918116j/article/details/48605595)》。我们在UIWebViewDelegate发现了四个方法。

```oc
__TVOS_PROHIBITED @protocol UIWebViewDelegate <NSObject>

@optional
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType;
- (void)webViewDidStartLoad:(UIWebView *)webView;
- (void)webViewDidFinishLoad:(UIWebView *)webView;
- (void)webView:(UIWebView *)webView didFailLoadWithError:(nullable NSError *)error;

@end
```

这四个代理的作用分别是：

- `- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType;`:将要加载一个web页面，返回yes代表继续加载，no代表不加载。
- `- (void)webViewDidStartLoad:(UIWebView *)webView;`:开始加载web页面；
- `- (void)webViewDidFinishLoad:(UIWebView *)webView;`:加载web页面结束;
- `- (void)webView:(UIWebView *)webView didFailLoadWithError:(nullable NSError *)error;`:加载web页面出错，你可以在error看到错误信息。

现在我们改造BaseVC。下面都只显示核心代码，不重要的代码不显示。

```oc
@interface BaseVC () <UIWebViewDelegate>

@property (weak, nonatomic) IBOutlet UIWebView *webView; ///< UIWebView

@end

@implementation BaseVC

- (void)viewDidLoad {
    [super viewDidLoad];
    // 找到index.html的路径
    NSURL *url = [[NSBundle mainBundle] URLForResource:@"index" withExtension:@"html"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url]; // url的位置
    self.webView.delegate = self;       // 代理UIWebViewDelegate
    [self.webView loadRequest:urlRequest]; // 加载页面
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
    return YES;
}

@end
```

运行后你会看到如下的输出,也体现了三种回调的顺序。如果你看到了其他输出信息，我想你可能出错了，请查阅前面的介绍，修改后再运行。

```
webView:shouldStartLoadWithRequest:navigationType:
webViewDidStartLoad:
webViewDidFinishLoad:
```

&#160;

#JS和OC互动

js和oc互动是一件很麻烦的事，毕竟是两种不同的开发语言。oc调js很简单，js回调oc就比较麻烦了。

- oc调js:uiwebview就自带这样的方法`- (nullable NSString *)stringByEvaluatingJavaScriptFromString:(NSString *)script;`
- js掉oc：我们都知道js可以控制页面的跳转，我们还可以在链接中携带参数。而UIWebView又支持拦截这些请求的操作，这就让我们完美的实现了js调oc的操作。至于更高级的方法，我们在以后给大家介绍。

改变原有页面的功能，我们要实现下列需求。

1. 在输入框输入相关信息，点击按钮，将输入的信息传输到oc中；
2. oc接受到信息后，通过oc调用js的功能将信息发到js；
3. js收到信息后，在页面显示相关信息。

由于js调用oc是通过url请求发送消息，所以，我们需要制定规范。只有符合我们规范的请求才会执行oc方法，否则不执行。

这里我使用的规范是`ios::oc方法名::携带的参数`。这样我们在oc端，当发现这样的请求时就开始拦截执行我们的操作。
我这里的三个参数，相信大家都能看懂，我是使用“::”分割，你也可以使用其他方式分割，或者组合你喜欢的规范。

接下来就是改造oc和js了。在BaseVC添加oc接受js调用的方法体，和改写`- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType`。

```oc
#pragma mark - js调OC
- (void)jsCallOC:(NSString *)params {
    NSLog(@"%@:%@", NSStringFromSelector(_cmd), params);
    NSString *jsStr = [NSString stringWithFormat:@"ocCallJS('%@')", params];
    // oc调js
    [self.webView stringByEvaluatingJavaScriptFromString:jsStr];
}

#pragma mark 网页监听
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    // 过滤
    NSString *requestString = request.URL.absoluteString.stringByRemovingPercentEncoding;
    // 分割
    NSArray *urlComps = [requestString componentsSeparatedByString:@"::"];
    if ([urlComps count] == 3 && [@"ios" isEqualToString:[urlComps objectAtIndex:0]])
    {
        //解析约定的指令
        // 方法名
        NSString *methods = [NSString stringWithFormat:@"%@:", [urlComps objectAtIndex:1]];
        SEL selector = NSSelectorFromString(methods);
        // 判断类是否有方法
        if ( [BaseVC instancesRespondToSelector:selector]) {
            // 携带的参数
            NSString *params = [urlComps objectAtIndex:2];
            NSLog(@"JS调用OC代码->UIWebView\n方法名:%@,参数:%@", methods, params);
            // 执行方法，携带参数
            [self performSelector:selector withObject:params];
        } else {
            NSLog(@"没有提供调用的%@方法名",methods);
        }
        return NO;
    }
    return YES;
}
```

`[self performSelector:selector withObject:params];`这段代码的意思是执行当前页面的方法体，并携带参数。我们使用通配的方式调用方法体，使代码更优雅便捷。

然后改变js代码。

```javascript
<script>
    // $解析器
    function $ (ele) {
        return document.querySelector(ele);
    };

    // 点击确定按钮
    function onClickButton() {
        // show.innerHTML = input.value;
        jsCallOC();
    }

    // JS调用UIWebView
    function jsCallOC() {
        // 	方法名
        var method = "jsCallOC";
        // 组装请求数据
        var url = "ios::" + method + "::" + input.value;
        // 发起请求
        document.location = url;
    }

    // UIWebView调用JS
    function ocCallJS(params) {
        show.innerHTML = params;
    }
</script>
```

运行项目，然后神奇的事情发生了，原来在app中嵌入网页这么简单。

&#160;

----------

#其他

##参考资料

 [UIWebView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIWebView_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-3 | 根据Objective-C中的UIWebView API总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j