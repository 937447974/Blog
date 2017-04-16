ACAccount记录账户相关信息。

# 1 Initializing Accounts

```swift
// 初始化
public init!(accountType type: ACAccountType!)
```

# 2 Accessing Properties

```swift
// 唯一标示符
public var identifier: String? { get }
    
// 账户类型
public var accountType: ACAccountType!
    
// 描述信息
public var accountDescription: String!
    
// 昵称
public var username: String!
    
// 全称
@available(iOS 7.0, *)
public var userFullName: String! { get }
    
// 身份信息
public var credential: ACAccountCredential!
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Accounts Framework Reference](https://developer.apple.com/library/ios/documentation/Accounts/Reference/AccountsFrameworkRef/index.html)

[ACAccount Class Reference](https://developer.apple.com/library/ios/documentation/Accounts/Reference/ACAccountClassRef/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog