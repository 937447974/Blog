#1 SFContentBlockerManager

SFContentBlockerManager通知SFSafariViewController加载内容过滤规则。

##1.1 Reloading Your Content Blocking Rules

```swift
/// 刷新扩展
///
/// - parameter identifier : 当前应用的bundle Identifier
/// - parameter completionHandler : 刷新后的回调
///
/// - returns: void
public class func reloadContentBlockerWithIdentifier(identifier: String, completionHandler: ((NSError?) -> Void)?)
```

#2 实战演练

下面显示刷新的核心代码。

```swift
SFContentBlockerManager.reloadContentBlockerWithIdentifier(YJUtilsAPP.identifier) { (error: NSError?) -> Void in
    if error != nil {
        print(error)
    }
}
```

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