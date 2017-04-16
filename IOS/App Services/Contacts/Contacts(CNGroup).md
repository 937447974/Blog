CNGroup能将联系人分组，这样有便于用户查看和分类整理联系人。

# 1 Group Properties

```swift
/// 唯一标示符
public var identifier: String { get }
/// 组名    
public var name: String { get }
```

# 2 Predicate Methods

```swift
/// 通过组id获取组
public class func predicateForGroupsWithIdentifiers(identifiers: [String]) -> NSPredicate

/// 通过容器id获取组
public class func predicateForGroupsInContainerWithIdentifier(containerIdentifier: String) -> NSPredicate
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Contacts Framework Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/Contacts_Framework/index.html)

[CNGroup Class Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/CNGroup_Class/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-14 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog