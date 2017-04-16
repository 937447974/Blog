1. [Identification Properties](#Identification_Properties)
2. [Date Properties](#Date_Properties)
3. [Name Properties](#Name_Properties)
4. [Localization](#Localization)
5. [Contact Comparator](#Contact_Comparator)
6. [Social Properties](#Social_Properties)
7. [Contact phone Number](#Contact_phone_Number)
8. [Url Address](#Url_Address)
9. [Postal Addresses](#Postal_Addresses)
10. [Email Address](#Email_Address)
11. [Note Property](#Note_Property)
12. [Image Properties](#Image_Properties)
13. [Labeled Value Properties](#Labeled_Value_Properties)
14. [Partial Contact property availability](#Partial_Contact_property_availability)
15. [CNContact Predicates](#CNContact_Predicates)

----

CNContact代表一个不可变的联系人，它是线程安全的，当我们想要修改某个联系人的数据时，可通过其子类CNMutableContact实现。

# <a id="Identification_Properties">1 Identification Properties

```swift
/// 唯一标示符
public var identifier: String { get }
/// 联系方式类型
public var contactType: CNContactType { get }
/// 判断唯一标示符是否唯一
public func isUnifiedWithContactWithIdentifier(contactIdentifier: String) -> Bool
```

# <a id="Date_Properties">2 Date Properties

```swift
/// 公历生日
@NSCopying public var birthday: NSDateComponents? { get }
/// 阴历生日
@NSCopying public var nonGregorianBirthday: NSDateComponents? { get }
/// 多个公历日期
public var dates: [CNLabeledValue] { get }
```

# <a id="Name_Properties">3 Name Properties

```swift
/// 姓名前缀
public var namePrefix: String { get }
/// 名
public var givenName: String { get }
/// 中间名
public var middleName: String { get }
/// 姓
public var familyName: String { get }
/// 姓前缀
public var previousFamilyName: String { get }
/// 名后缀
public var nameSuffix: String { get }
/// 绰号
public var nickname: String { get }
    
/// 语音名
public var phoneticGivenName: String { get }
/// 语音中间名
public var phoneticMiddleName: String { get }
/// 语音姓
public var phoneticFamilyName: String { get }
    
/// 公司
public var organizationName: String { get }
/// 部门
public var departmentName: String { get }
/// 职位
public var jobTitle: String { get }
```

# <a id="Localization">4 Localization

```swift
/// 获取本地化显示
public class func localizedStringForKey(key: String) -> String
```

# <a id="Contact_Comparator">5 Contact Comparator

```swift
/// 根据制定类型排序
public class func comparatorForNameSortOrder(sortOrder: CNContactSortOrder) -> NSComparator

/// 获取排序关键字
public class func descriptorForAllComparatorKeys() -> CNKeyDescriptor
```

# <a id="Social_Properties">6 Social Properties

```swift
/// 社交关系
public var socialProfiles: [CNLabeledValue] { get }
```

# <a id="Contact_phone_Number">7 Contact phone Number

```swift
/// 电话号码
public var phoneNumbers: [CNLabeledValue] { get }
```

# <a id="Url_Address">8 Url Address

```swift
/// 网络地址
public var urlAddresses: [CNLabeledValue] { get }
```

# <a id="Postal_Addresses">9 Postal Addresses

```swift
/// 家庭地址
public var postalAddresses: [CNLabeledValue] { get }
```

# <a id="Email_Address">10 Email Address

```swift
/// 邮箱地址
public var emailAddresses: [CNLabeledValue] { get }
```

# <a id="Note_Property">11 Note Property

```swift
// 备注
public var note: String { get }
```

# <a id="Image_Properties">12 Image Properties

```swift
/// 头像
@NSCopying public var imageData: NSData? { get }
/// 头像缩略图
@NSCopying public var thumbnailImageData: NSData? { get }
/// 校验头像数据是否有效
@available(iOS 9.0, *)
public var imageDataAvailable: Bool { get }
```

# <a id="Labeled_Value_Properties">13 Labeled Value Properties

```swift
/// 链接已有联系人
public var contactRelations: [CNLabeledValue] { get }
/// 即时通讯地址
public var instantMessageAddresses: [CNLabeledValue] { get }
```

# <a id="Partial_Contact_property_availability">14 Partial Contact property availability

```swift
/// 通过制定的key获取信息时，判断能否获取成功
public func isKeyAvailable(key: String) -> Bool
    
/// 判断CNKeyDescriptor是否有效
public func areKeysAvailable(keyDescriptors: [CNKeyDescriptor]) -> Bool
```

# <a id="CNContact_Predicates">15 CNContact Predicates

```swift
/// 通过姓名匹配获取联系人
public class func predicateForContactsMatchingName(name: String) -> NSPredicate

/// 通过联系人唯一标示符，获取联系人
public class func predicateForContactsWithIdentifiers(identifiers: [String]) -> NSPredicate

/// 通过组id获取联系人
public class func predicateForContactsInGroupWithIdentifier(groupIdentifier: String) -> NSPredicate

/// 通过联系人容器标示符获取联系人
public class func predicateForContactsInContainerWithIdentifier(containerIdentifier: String) -> NSPredicate
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Contacts Framework Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/Contacts_Framework/index.html)

[CNContact Class Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/CNContact_Class/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-14 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog