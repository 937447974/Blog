ACAccountCredential记录账户的身份信息。

#1 Initializing Credentials

```swift
// 初始化ACAccountCredential
public init!(OAuthToken token: String!, tokenSecret secret: String!)
    
// 初始化ACAccountCredential
public init!(OAuth2Token token: String!, refreshToken: String!, expiryDate: NSDate!)
```

#2 Accessing Credential Properties

```swift
// OAuth2证书token
public var oauthToken: String!
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Accounts Framework Reference](https://developer.apple.com/library/ios/documentation/Accounts/Reference/AccountsFrameworkRef/index.html)

[ACAccountCredential Class Reference](https://developer.apple.com/library/ios/documentation/Accounts/Reference/ACAccountCredentialClassRef/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-26 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog