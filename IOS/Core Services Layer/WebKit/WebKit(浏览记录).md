[WebKit(展示Web界面)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(展示Web界面).md)

[WebKit(WKScriptMessageHandler)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(WKScriptMessageHandler).md)

[WebKit(WKUIDelegate)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(WKUIDelegate).md)

[WebKit(WKNavigationDelegate)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(WKNavigationDelegate).md)

[WebKit(刷新)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(刷新).md)

[WebKit(导航)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(导航).md)

[WebKit(浏览记录)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(浏览记录).md)

[WebKit(进度条)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/WebKit/WebKit(进度条).md)

------

当我们在百度首页进行相应的浏览时，会产生浏览记录，也可以理解为历史记录。我们可以通过浏览记录快速进入我们想要进入的页面。

在WebKit中有WKBackForwardList类，就是专门记录浏览记录的。

#1 WKBackForwardList相关属性

打开WKBackForwardList，会看见如下属性，

```objc
// 当前网页对象
@property (nullable, nonatomic, readonly, strong) WKBackForwardListItem *currentItem;

// 上一个网页所在对象
@property (nullable, nonatomic, readonly, strong) WKBackForwardListItem *backItem;

// 上一个网页所在对象
@property (nullable, nonatomic, readonly, strong) WKBackForwardListItem *forwardItem;

// 通过位置获取网页对象
- (nullable WKBackForwardListItem *)itemAtIndex:(NSInteger)index;

// 回退的历史网页
@property (nonatomic, readonly, copy) NSArray<WKBackForwardListItem *> *backList;

// 前进的历史网页
@property (nonatomic, readonly, copy) NSArray<WKBackForwardListItem *> *forwardList;
```

#2 浏览记录

##2.1 创建查询按钮

浏览记录用查询按钮，改写initUIBarButtonItem()方法添加查询按钮。

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
    UIBarButtonItem *searchItem = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemSearch target:self action:@selector(searchBackForwardList:)];
    UIBarButtonItem *reloadItem = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemRefresh target:self action:@selector(reload:)];
    self.navigationItem.rightBarButtonItems = [NSArray arrayWithObjects:reloadItem, searchItem, nil];
}
```

##2.2 实现查询方法

实现查询方法使用searchBackForwardList：。

```objc
#pragma mark 浏览记录
- (void)searchBackForwardList:(id)sender {
    // 查询历史记录
    WKBackForwardList *backForwardList = self.webView.backForwardList;
    // 历史
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"浏览记录" message:nil preferredStyle:UIAlertControllerStyleActionSheet];
    __weak WKWebView *webView = self.webView;
    // 后退
    for (__weak WKBackForwardListItem *backItem in backForwardList.backList) {
        [alertController addAction:[UIAlertAction actionWithTitle:backItem.title style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            [webView goToBackForwardListItem:backItem];
        }]];
    }
    // 当前页面
    [alertController addAction:[UIAlertAction actionWithTitle:backForwardList.currentItem.title style:UIAlertActionStyleDestructive handler:^(UIAlertAction * _Nonnull action) {
        [webView reload];
    }]];
    // 前进
    for (__weak WKBackForwardListItem *forwardItem in backForwardList.forwardList) {
        [alertController addAction:[UIAlertAction actionWithTitle:forwardItem.title style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            [webView goToBackForwardListItem:forwardItem];
        }]];
    }
    // 取消按钮
    [alertController addAction:[UIAlertAction actionWithTitle:@"Cancel" style:UIAlertActionStyleCancel handler:nil]];
    
    // 显示
    [self presentViewController:alertController animated:YES completion:nil];
}
```

在这里我们使用UIAlertController的UIAlertControllerStyleActionSheet样式展示历史记录，你可以点击不同的条目进入不同的网页。
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
| 2015-12-12 | 更改链接 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog