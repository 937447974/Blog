[WebKit(展示Web界面)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(展示Web界面).md)

[WebKit(WKScriptMessageHandler)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKScriptMessageHandler).md)

[WebKit(WKUIDelegate)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKUIDelegate).md)

[WebKit(WKNavigationDelegate)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKNavigationDelegate).md)

[WebKit(刷新)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(刷新).md)

[WebKit(导航)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(导航).md)

[WebKit(浏览记录)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(浏览记录).md)

[WebKit(进度条)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(进度条).md)

------

作为一款模拟浏览器的项目，必然要支持界面刷新。其实刷新很简单，WekWebView就支持相关功能。

#1 WekWebView刷新相关

在WekWebView有一个属性和两个方法管理刷新。

```objc
// 是否正在刷新
@property (nonatomic, readonly, getter=isLoading) BOOL loading;
// 刷新界面
- (nullable WKNavigation *)reload;
// 停止刷新
- - (void)stopLoading;
```

#2 实现刷新

##2.1 创建刷新按钮

我们将刷新功能用按钮实现，将其添加到UINavigationController导航上。

添加方法initUIBarButtonItem()。

```objc
#pragma mark 初始化UIBar导航按钮
- (void)initUIBarButtonItem {
    // 右边
    UIBarButtonItem *reloadItem = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemRefresh target:self action:@selector(reload:)];
    self.navigationItem.rightBarButtonItem = searchItem;
}
```

##2.2 加载刷新按钮

在viewDidLoad()中使用

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    [self initUIBarButtonItem];
    // 刷新界面
    NSURL *url = [NSURL URLWithString:@"https://www.baidu.com"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url];
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

##2.3 实现刷新方法

接下来实现刷新方法reload:。

```objc
#pragma mark 刷新
- (void)reload:(id)sender {
    if (self.webView.loading) { // 是否正在刷新页面
        [self.webView stopLoading]; // 停止刷新
    }
    // 刷新页面
    [self.webView reload];
}
```

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

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog