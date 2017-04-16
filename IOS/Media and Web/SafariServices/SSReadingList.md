# 1 SSReadingList

SSReadingList能够帮助我们将网络连接添加到Safari浏览器的阅读列表。

## 1.1 Getting the Reading List Singleton Object

```swift
/// 获取SSReadingList
public class func defaultReadingList() -> SSReadingList?
```

## 1.2 Checking Whether a URL is Supported by Reading List

```swift
/// 判断url能否添加到Safari浏览器的阅读列表
public class func supportsURL(URL: NSURL) -> Bool
```

## 1.3 Adding an Item to the Reading List

```swift
/// 添加url到Safari浏览器的阅读列表
///
/// - parameter URL : 网络连接
/// - parameter title : 标题
/// - parameter previewText : 描述
///
/// - returns: void
@available(iOS 7.0, *)
public func addReadingListItemWithURL(URL: NSURL, title: String?, previewText: String?) throws
```

# 2 实战演练

核心代码

```swift
guard let url = NSURL(string: "https://www.baidu.com") else { // 生成url
    return
}
guard SSReadingList.supportsURL(url) else { // 是否支持添加该链接
    return
}
do {
    // 添加到阅读列表
    try SSReadingList.defaultReadingList()?.addReadingListItemWithURL(url, title: "title", previewText: "previewText")
} catch SSReadingListErrorCode.URLSchemeNotAllowed {
    print(SSReadingListErrorDomain)
} catch {
    print(error)
}
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Safari Services Framework Reference](https://developer.apple.com/library/ios/documentation/SafariServices/Reference/SafariServicesFramework_Ref/index.html)

[SSReadingList Class Reference](https://developer.apple.com/library/ios/documentation/UserExperience/Reference/SSReadingList_Ref/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-21 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog