[WebKit(展示Web界面)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(展示Web界面).md)

[WebKit(WKScriptMessageHandler)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKScriptMessageHandler).md)

[WebKit(WKUIDelegate)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKUIDelegate).md)

[WebKit(WKNavigationDelegate)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKNavigationDelegate).md)

[WebKit(刷新)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(刷新).md)

[WebKit(导航)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(导航).md)

[WebKit(浏览记录)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(浏览记录).md)

[WebKit(进度条)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(进度条).md)

------

在上一篇博文《[WebKit(刷新)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(刷新).md)》讲解了刷新网页。接下来讲解网页前进和后退功能。

#1 WekWebView导航相关

在WekWebView中有管理页面前进后后退的属性和方法。

```objc
// 能否回到上一页
@property (nonatomic, readonly) BOOL canGoBack;

// 回调上一页
- (nullable WKNavigation *)goBack;

// 能否去往下一页
@property (nonatomic, readonly) BOOL canGoForward;

// 跳转到下一页
- (nullable WKNavigation *)goForward;
```

#2 实现导航

##2.1 强引用导航按钮

我们还是在UINavigationController添加相关按钮。

考虑到没有上一页和下一页时，给用户更好的视觉感受，导航按钮不可点。因此我们添加强引用的导航按钮。

```objc
@property (nonatomic, strong) UIBarButtonItem *goBackBarButtonItem;    ///< 上一页按钮
@property (nonatomic, strong) UIBarButtonItem *goForwardBarButtonItem; ///< 下一页按钮
```

##2.2 创建导航按钮

改造initUIBarButtonItem()方法。

```objc
#pragma mark 初始化UIBar导航按钮
- (void)initUIBarButtonItem {
    // 左边
    self.goBackBarButtonItem = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemRewind target:self action:@selector(goBack:)];
    self.goBackBarButtonItem.enabled = NO; // 不可点
    self.goForwardBarButtonItem = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemFastForward target:self action:@selector(goForward:)];
    self.goForwardBarButtonItem.enabled = NO;
    self.navigationItem.leftBarButtonItems = [NSArray arrayWithObjects:self.goBackBarButtonItem, self.goForwardBarButtonItem, nil];
    // 右边
    UIBarButtonItem *reloadItem = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemRefresh target:self action:@selector(reload:)];
    self.navigationItem.rightBarButtonItems = [NSArray arrayWithObjects:reloadItem, nil];
}
```

##2.3 实现导航方法

在导航的时候，为了安全，我们先判断一下能否跳转，再执行跳转操作。

```objc
#pragma mark 上一页
- (void)goBack:(id)sender {
    if ([self.webView canGoBack]) {
        [self.webView goBack];
    }
}

#pragma mark 下一页
- (void)goForward:(id)sender {
    if ([self.webView canGoForward]) {
        [self.webView goForward];
    }
}
```

##2.4 导航按钮能否点击

为了给用户更好的感觉，我们应该让用户直观的感觉到按钮能否点击。

改写WKNavigationDelegate协议的`- (void)webView:(WKWebView *)webView didFinishNavigation:(null_unspecified WKNavigation *)navigation`方法。

```objc
#pragma mark 页面加载完成之后调用
- (void)webView:(WKWebView *)webView didFinishNavigation:(null_unspecified WKNavigation *)navigation {
    NSLog(@"%s",__FUNCTION__);
    if (!self.webView.loading) {
        self.navigationItem.title = webView.title; // 页面标题
        // 上一页按钮和下一页按钮是否可点
        self.goBackBarButtonItem.enabled = [self.webView canGoBack];
        self.goForwardBarButtonItem.enabled = [self.webView canGoForward];
    }
}
```

在这里当页面加载完成时，还同时将网页的标题设为navigationItem.title。

运行项目即可测试导航效果。
&#160;

----------

#其他

##源代码

[Objective-C](https://github.com/937447974/Objective-C)

##参考资料

[WebKit Framework Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/WebKit/ObjC_classic/index.html#//apple_ref/doc/uid/TP30000745)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-02 | 博文完成 |
| 2015-12-12 | 更改链接 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog