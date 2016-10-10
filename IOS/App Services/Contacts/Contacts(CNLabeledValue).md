CNLabeledValue是线程安全的，用于记录联系人内的多维数据，如电话、家庭地址、邮件等。

#1 Setting Identifiers

```swift
/// 唯一标示符
public var identifier: String { get }
/// 初始化
public init(label: String?, value: protocol<NSCopying, NSSecureCoding>)
```

#2 Setting Labels and Values

```swift
/// 设置label，返回新CNLabeledValue
public func labeledValueBySettingLabel(label: String?) -> Self
    
/// 设置value，返回新CNLabeledValue
public func labeledValueBySettingValue(value: protocol<NSCopying, NSSecureCoding>) -> Self
    
/// 设置label和value，返回新CNLabeledValue
public func labeledValueBySettingLabel(label: String?, value: protocol<NSCopying, NSSecureCoding>) -> Self
```

#3 Labels and Values

```swift
/// 键
public var label: String { get }
/// 值
@NSCopying public var value: protocol<NSCopying, NSSecureCoding> { get }
```

#4 Localization

```swift
/// 格式化label获取本地描述
public class func localizedStringForLabel(label: String) -> String
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Contacts Framework Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/Contacts_Framework/index.html)

[CNLabeledValue Class Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/CNLabeledValue_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-14 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog