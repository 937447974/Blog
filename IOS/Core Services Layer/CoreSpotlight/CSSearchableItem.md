CSSearchableItem代表搜索栏中的每一个条目。配合CSSearchableIndex保存数据到搜索栏，配合CSSearchableItemAttributeSet封装数据。

#1 Getting a Searchable Item

```swift
// 初始化CSSearchableItem
public init(uniqueIdentifier: String?, domainIdentifier: String?, attributeSet: CSSearchableItemAttributeSet)
```

#2 Setting Attributes on a Searchable Item

```
// 唯一标示符
public var uniqueIdentifier: String
    
// item的所有者，可以理解为item的组id
public var domainIdentifier: String?
    
// 可搜索的items失效时间，默认一个月
@NSCopying public var expirationDate: NSDate!
    
// 设置itme attributes包含的元数据
public var attributeSet: CSSearchableItemAttributeSet
```

&#160;

----

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Core Spotlight Framework Reference](https://developer.apple.com/library/ios/documentation/CoreSpotlight/Reference/CoreSpotlight_Framework/index.html)

[App Search Programming Guide](https://developer.apple.com/library/ios/documentation/General/Conceptual/AppSearch/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-28 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog