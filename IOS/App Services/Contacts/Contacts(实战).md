Contacts Framework主要用于帮助我们获取用户的联系方式，即通讯录中的相关信息。我们可以对其实现增删改查及归纳整理等相关功能。

下面介绍一些比较常用的代码实现。

#1 增加联系人

```swift
// Creating a mutable object to add to the contact
let contact = CNMutableContact()
contact.contactType = CNContactType.Person // 类型
// 照片
if let image = UIImage(named: "qq") {
    if let data = UIImagePNGRepresentation(image) {
        contact.imageData = data
    }
}
contact.familyName = "阳" // 姓
contact.givenName = "君" // 名
contact.jobTitle = "IOS攻城师" // 工作
// 电话
let homePhone = CNLabeledValue(label:CNLabelPhoneNumberMobile, value:CNPhoneNumber(stringValue:"185-1105-6826"))
contact.phoneNumbers = [homePhone]
    
// 邮件
let workEmail = CNLabeledValue(label:CNLabelWork, value:"937447974@qq.com") // 工作
contact.emailAddresses = [workEmail]
    
// 家庭地址
let homeAddress = CNMutablePostalAddress()
homeAddress.street = "西城区" // 街道
homeAddress.city = "北京" // 城市
homeAddress.state = "北京"// 省
homeAddress.postalCode = "000000" // 邮编
contact.postalAddresses = [CNLabeledValue(label:CNLabelHome, value:homeAddress)] // 添加地址
    
// 生日
let birthday = NSDateComponents()
birthday.year = 2016
birthday.month = 1
birthday.day = 12
contact.birthday = birthday
    
// Saving the newly created contact
do {
    let saveRequest = CNSaveRequest()
    saveRequest.addContact(contact, toContainerWithIdentifier:self.store.defaultContainerIdentifier())
    try self.store.executeSaveRequest(saveRequest)
} catch {
    print("未知错误：\(error)")
}
```

#2 查询联系人

```swift
/// 快速查询
private var predicate: NSPredicate! {
    didSet {
        do {
            self.data = try self.store.unifiedContactsMatchingPredicate(self.predicate, keysToFetch:[CNContactGivenNameKey, CNContactFamilyNameKey])
            self.tableView.reloadData()
        } catch {
            print("未知错误：\(error)")
        }
    }
}

// 调用
self.predicate = CNContact.predicateForContactsInContainerWithIdentifier(self.store.defaultContainerIdentifier()) //predicateForContactsMatchingName("")
```

#3 删除联系人

```swift
// 删除电话
do {
    // 删除通讯录中电话
    let saveRequest = CNSaveRequest()
    let contact = self.data[indexPath.row].mutableCopy() as! CNMutableContact
    saveRequest.deleteContact(contact)
    // 保存
    try self.store.executeSaveRequest(saveRequest)
} catch {
    print("未知错误：\(error)")
}
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Contacts Framework Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/Contacts_Framework/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-14 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog