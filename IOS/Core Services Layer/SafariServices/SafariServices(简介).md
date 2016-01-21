当我们想在app中使用网页时以前有两种方案，一种是跳到Safari App；另一种就是自己搭建VC使用WKWebView或UIWebView显示网页。

现在有了第三种方案，使用SafariServices库，它能帮助我们在App中快速继承Safari的界面而无需跳转到Safari App。

#Classes

- NSObject
    - SFContentBlockerManager 通知Safari刷新内容屏蔽规则。
    - SSReadingList 添加URL链接到Safari浏览器的阅读列表。

- UIViewController
    - SFSafariViewController 浏览Web的界面。

# Protocols

- SFSafariViewControllerDelegate 监听SFSafariViewController生命周期和添加共享按钮。

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Safari Services Framework Reference](https://developer.apple.com/library/ios/documentation/SafariServices/Reference/SafariServicesFramework_Ref/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-21 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog