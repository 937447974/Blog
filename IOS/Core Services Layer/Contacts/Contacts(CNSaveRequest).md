当使用CNContactStore保存通讯录时，需要使用CNSaveRequest记录通讯录的变动，即修改、增加等操作。

#1 Saving a Contact Changes

```swift
/// 添加联系人
public func addContact(contact: CNMutableContact, toContainerWithIdentifier identifier: String?)

/// 修改联系人
public func updateContact(contact: CNMutableContact)

/// 删除联系人
public func deleteContact(contact: CNMutableContact)
```

#2 Saving Group Changes

```swift
/// 添加分组
public func addGroup(group: CNMutableGroup, toContainerWithIdentifier identifier: String?)
 
/// 修改分组
public func updateGroup(group: CNMutableGroup)

/// 删除分组
public func deleteGroup(group: CNMutableGroup)

/// 添加联系人到组中
public func addMember(contact: CNContact, toGroup group: CNGroup)
 
/// 从组中删除联系人
public func removeMember(contact: CNContact, fromGroup group: CNGroup)
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Contacts Framework Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/Contacts_Framework/index.html)

[CNContactStore Class Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/CNContactStore_Class/index.html)

[CNSaveRequest Class Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/CNSaveRequest_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-14 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog