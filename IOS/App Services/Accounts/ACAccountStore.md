ACAccountStore管理账户授权及对账户ACAccount的增、删、改和查。

# 1 Getting Accounts

```swift
/// 获取所有账户
public var accounts: [AnyObject]? { get }
    
/// 根据账户标示符获取账户
public func accountWithIdentifier(identifier: String!) -> ACAccount!
    
/// 根据账户类型获取账户
public func accountsWithAccountType(accountType: ACAccountType!) -> [AnyObject]!
```

# 2 Getting Account Types

```swift
/// 获取账户类型，如获取微博，传入ACAccountTypeIdentifierSinaWeibo
public func accountTypeWithAccountTypeIdentifier(typeIdentifier: String!) -> ACAccountType!
```

# 3 Saving Accounts

```swift
/// 保存账户
public func saveAccount(account: ACAccount!, withCompletionHandler completionHandler: ACAccountStoreSaveCompletionHandler!)
```

# 4 Requesting Access

```swift
// 用户授权
public func requestAccessToAccountsWithType(accountType: ACAccountType!, options: [NSObject : AnyObject]!, completion: ACAccountStoreRequestAccessCompletionHandler!)
```

# 5 Renewing Account Credentials

```swift
// 更新账户
public func renewCredentialsForAccount(account: ACAccount!, completion completionHandler: ACAccountStoreCredentialRenewalHandler!)
```

# 6 Removing Accounts

```swift
// 删除账户
public func removeAccount(account: ACAccount!, withCompletionHandler completionHandler: ACAccountStoreRemoveCompletionHandler!)
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Accounts Framework Reference](https://developer.apple.com/library/ios/documentation/Accounts/Reference/AccountsFrameworkRef/index.html)

[ACAccountStore Class Reference](https://developer.apple.com/library/ios/documentation/Accounts/Reference/ACAccountStoreClassRef/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog