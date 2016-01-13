CNContactStore主要用于沟通用户存储联系人的数据仓库，通过它可以对用户通讯录中的联系人进行查询、修改、增加等操作。

#1 Fetching Unified Contacts

```swift
/// 获取联系人
///
/// - parameter predicate: NSPredicate
/// - parameter keys: [CNKeyDescriptor]
///
/// - returns: void
public func unifiedContactsMatchingPredicate(predicate: NSPredicate, keysToFetch keys: [CNKeyDescriptor]) throws -> [CNContact]

/// 通过唯一标示符获取联系人
///
/// - parameter identifier : String
/// - parameter keys: 要获取相关内容
///
/// - returns: void
public func unifiedContactWithIdentifier(identifier: String, keysToFetch keys: [CNKeyDescriptor]) throws -> CNContact
```

#2 Privacy Access

```swift
/// 用户认证的权限
///
/// - parameter entityType: CNEntityType.Contacts
///
/// - returns: CNAuthorizationStatus
public class func authorizationStatusForEntityType(entityType: CNEntityType) -> CNAuthorizationStatus

/// 权限认证
///
/// - parameter entityType: CNEntityType.Contacts
/// - parameter completionHandler: 闭包回调认证结果
///
/// - returns: void
public func requestAccessForEntityType(entityType: CNEntityType, completionHandler: (Bool, NSError?) -> Void)
```

#3 Fetching and Saving

```swift
/// 闭包获取联系人
///
/// - parameter fetchRequest : CNContactFetchRequest查询条件
/// - parameter block : 闭包输出
///
/// - returns: void
public func enumerateContactsWithFetchRequest(fetchRequest: CNContactFetchRequest, usingBlock block: (CNContact, UnsafeMutablePointer<ObjCBool>) -> Void) throws

/// 获取分组
///
/// - parameter predicate : NSPredicate?
///
/// - returns : [CNGroup]
public func groupsMatchingPredicate(predicate: NSPredicate?) throws -> [CNGroup]

/// 获取容器
///
/// - parameter predicate : NSPredicate?
///
/// - returns : [CNContainer]
public func containersMatchingPredicate(predicate: NSPredicate?) throws -> [CNContainer]

/// 保存联系人
///
/// - parameter saveRequest : CNSaveRequest联系人修改记录
///
/// - returns: void
public func executeSaveRequest(saveRequest: CNSaveRequest) throws
    
/// 获取容器唯一标示符
///
/// - returns : String
public func defaultContainerIdentifier() -> String
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Contacts Framework Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/Contacts_Framework/index.html)

[CNContactStore Class Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/CNContactStore_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-13 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog