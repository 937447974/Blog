[WebKit(展示Web界面)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(展示Web界面).md)

[WebKit(WKScriptMessageHandler)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKScriptMessageHandler).md)

[WebKit(WKUIDelegate)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKUIDelegate).md)

[WebKit(WKNavigationDelegate)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(WKNavigationDelegate).md)

[WebKit(刷新)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(刷新).md)

[WebKit(导航)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(导航).md)

[WebKit(浏览记录)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(浏览记录).md)

[WebKit(进度条)](https://github.com/937447974/Blog/blob/master/WebKit/WebKit(进度条).md)

------

仿照qq或微信的进度条，我们也为我们的浏览器添加进度条功能。

#1 WKWebView相关属性

在WKWebView就有一个关于网页加载进度的属性。

```objc
// 网络加载进度，0~1
@property (nonatomic, readonly) double estimatedProgress;
```

我们所要做的是使用KVO监听这个属性，然后显示到进度条上。

#2 创建进度条

这里我们就是使用控件UIProgressView创建一个进度条，你还可以自行设计更精美的进度条。

##2.1 强引用进度条

由于我们需要时刻更新进度条的数据，故我们使用强引用，设置进度条。

```objc
@property (nonatomic, strong) UIProgressView *progressView; ///< 进度条
```

##2.2 懒加载进度条

我们使用懒加载的方式加载进度条。实现get方法。

```objc
- (UIProgressView *)progressView {
    if (_progressView == nil) {
        CGRect rect = CGRectZero;
        rect.origin.y = self.navigationController.navigationBar.frame.origin.y + self.navigationController.navigationBar.frame.size.height;
        rect.size.width = UIScreenWeight;
        rect.size.height = 2;
        _progressView = [[UIProgressView alloc] initWithFrame:rect];
        [_progressView setProgressViewStyle:UIProgressViewStyleDefault]; //设置进度条类型
        [self.view addSubview:_progressView];
    }
    return _progressView;
}
```

#3 KVO实现进度条

##3.1 KVO监听

在viewDidLoad()中，我们监听WKWebView的estimatedProgress属性。

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    [self initUIBarButtonItem];
    // 进度条监视
    NSLog(@"%f", self.webView.estimatedProgress); // 防止苹果改变属性名时，项目不报错。故这里先打印。
    [self.webView addObserver:self forKeyPath:@"estimatedProgress" options:NSKeyValueObservingOptionNew context:nil];
    // 刷新界面
    NSURL *url = [NSURL URLWithString:@"https://www.baidu.com"];
    NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url];
    [self.webView loadRequest:urlRequest]; // 加载页面
}
```

##3.2 KVO回调实现

所有类的KVO回调都是实现`- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSString *,id> *)change context:(void *)context`方法。

添加方法。

```objc
#pragma mark - KVO
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSString *,id> *)change context:(void *)context {
    // 进度条
    if ([@"estimatedProgress" isEqualToString:keyPath]) {
        NSLog(@"%f", self.webView.estimatedProgress);
        [self.progressView setProgress:self.webView.estimatedProgress animated:YES];
        // 初始和终止状态
        if (self.progressView.progress == 0) {
            self.progressView.hidden = NO;
        } else if (self.progressView.progress == 1) {
            // 1秒后隐藏
            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                // 再次判断，防止正在加载时隐藏
                if (self.progressView.progress == 1) {
                    self.progressView.progress = 0;
                    self.progressView.hidden = YES;
                }
            });
        }
    }
}
```

在这里我们分别判断了初始和终止状态，在终止状态我们会1秒后隐藏progressView；初始状态再显示。

到这里WebKit框架的讲解就完毕了，希望给你带来帮助。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015120201.jpg)

&#160;

----------

#其他

##源代码

[Objective-C](https://github.com/937447974/Objective-C)

##参考资料

[WebKit Framework Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/WebKit/ObjC_classic/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-02 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog