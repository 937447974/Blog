
1. [Getting an Attribute Set](#1)
2. [Working with Custom Attributes](#2)
3. [Describing Documents](#3)
4. [Describing Events](#4)
5. [Describing General Attributes](#5)
6. [Describing Places](#6)
7. [Describing Media](#7)
8. [Describing Music](#8)
9. [Describing Images](#9)
10. [Describing Messages](#10)
11. [Describing Containment](#11)
12. [Supporting Actions](#12)

----

CSSearchableItemAttributeSet声明CSSearchableItem包含的元数据。

# <a id="1">1 Getting an Attribute Set

```swift
/// 初始化CSSearchableItemAttributeSet
///
/// - parameter itemContentType : 统一标签，如kUTTypePNG或kUTTypeMovie(import MobileCoreServices)
///
/// - returns: void
 public init(itemContentType: String)
```

# <a id="2">2 Working with Custom Attributes

```swift
/// 存储数据
///
/// - parameter value : 自定义属性，如NSString, NSNumber, NSNull, NSData, or NSDate, or an array
/// - parameter key : 属性keyCSCustomAttributeKey
///
/// - returns: void
public func setValue(value: NSSecureCoding?, forCustomKey key: CSCustomAttributeKey)

// 获取存储的数据
public func valueForCustomKey(key: CSCustomAttributeKey) -> NSSecureCoding?
```

# <a id="3">3 Describing Documents

```swift
// 主题
public var subject: String?
    
// 主题
public var theme: String?
    
// 内容
public var contentDescription: String?
    
// 标示符
public var identifier: String?
    
// 所有者
public var audiences: [String]?
    
// 文件大小
public var fileSize: NSNumber?
    
// 页面总数
public var pageCount: NSNumber?
    
// 页面款第，每英寸72点
public var pageWidth: NSNumber?
    
// 页面高度，每英寸72点
public var pageHeight: NSNumber?
    
// 文件加密
public var securityMethod: String?

// 创建文件，如Word
public var creator: String?
    
// 内容转换为pdf流
public var encodingApplications: [String]?
    
// 种类
public var kind: String?
    
// 字体名
public var fontNames: [String]?
```

# <a id="4">4 Describing Events

```swift
// 到期时间
public var dueDate: NSDate?
    
// 完成时间
public var completionDate: NSDate?
    
// 开始时间
public var startDate: NSDate?

// 结束时间
public var endDate: NSDate?
    
// 关联时间
public var importantDates: [NSDate]?
    
// 事件覆盖的天数
@NSCopying public var allDay: NSNumber?
```

# <a id="5">5 Describing General Attributes

# <a id="6">6 Describing Places

# <a id="7">7 Describing Media

# <a id="8">8 Describing Music

# <a id="9">9 Describing Images

# <a id="10">10 Describing Messages

# <a id="11">11 Describing Containment

# <a id="12">12 Supporting Actions

```swift
// 电话与item相连
@NSCopying public var supportsPhoneCall: NSNumber?
    
// 导航与itme相连
@NSCopying public var supportsNavigation: NSNumber?
```

&#160;

----

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Core Spotlight Framework Reference](https://developer.apple.com/library/ios/documentation/CoreSpotlight/Reference/CoreSpotlight_Framework/index.html)

[App Search Programming Guide](https://developer.apple.com/library/ios/documentation/General/Conceptual/AppSearch/index.html)

[CSSearchableItemAttributeSet Class Reference](https://developer.apple.com/library/ios/documentation/CoreSpotlight/Reference/CSSearchableItemAttributeSet_Class/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-28 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog