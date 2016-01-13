Contacts Framework主要用于帮助我们获取用户的联系方式，即通讯录中的相关信息。我们可以对其实现增删改查及归纳整理等相关功能。

#Classes

- NSObject
    - CNContactStore：联系人的存储库。
    - CNSaveRequest：保存联系人。
    - CNContainer：联系人容器。
    - CNGroup：联系人分组。
        - CNMutableGroup：修改联系人分组时使用。
    - CNContactFetchRequest：快速获取联系人。在`[CNContactStore enumerateContactsWithFetchRequest:error:usingBlock:]`中使用。
    - CNContact：联系人
        - CNMutableContact：要修改联系人的信息时使用。
    - CNContactProperty：联系人的相关属性
    - CNLabeledValue：联系人的相关属性对应的值。
    - CNContactRelation：联系人关联其他联系人。
    - CNContactVCardSerialization：名片
    - CNContactsUserDefaults
    - CNInstantMessageAddress：联系人地址
    - CNPhoneNumber：电话号码
    - CNPostalAddress：邮政地址。
        - CNMutablePostalAddress：修改邮政地址时使用。
    - CNSocialProfile：社会现象。
- NSFormatter
    - CNContactFormatter：格式化获取联系人相关信息。
    - CNPostalAddressFormatter：格式化获取联系人地址
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
| 2016-01-13 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog