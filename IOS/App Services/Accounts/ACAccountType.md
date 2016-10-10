ACAccountType记录账户类型，用于表示同类账户，如新浪微博的账户。

#Accessing Properties

```swift
// 描述信息
public var accountTypeDescription: String! { get }
    
// 唯一标示符
public var identifier: String! { get }
    
// 判断当前app是否有访问这个账户类型的权限
public var accessGranted: Bool { get }
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Accounts Framework Reference](https://developer.apple.com/library/ios/documentation/Accounts/Reference/AccountsFrameworkRef/index.html)

[ACAccountType Class Reference](https://developer.apple.com/library/ios/documentation/Accounts/Reference/ACAccountTypeClassRef/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-26 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog