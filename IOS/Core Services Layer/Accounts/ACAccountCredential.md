Accounts Framework能够帮助我们访问账户库中的账户信息。可以用做服务的身份验证，如新浪微博。也可以通过它创建和保存一个用户账户，通过存储账号即可快速实现登录功能。

目前支持的账户有Twitter、Facebook、新浪微博和腾讯微博。查看IOS 9的手机设置发现有Flickr和Vimeo、估计后续会支持这两个账户。

#Classes

- NSObject
    - ACAccount 用户账号信息。
    - ACAccountCredential 用户的权限验证信息，如token。
    - ACAccountStore ACAccountStore提供访问、修改和存储账号的接口。
    - ACAccountType 封装所有特定类型的账户信息。

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Accounts Framework Reference](https://developer.apple.com/library/ios/documentation/Accounts/Reference/AccountsFrameworkRef/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-26 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog