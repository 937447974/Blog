SLRequest对象封装了一个HTTP请求的属性，为你创建请求提供了一个方便的模板。你可以发送一个请求至社交网络服务中，来代表用户执行一些操作或者获取用户信息。

HTTP请求有一些通用的组件：一个HTTP请求的方法(GET，POST，DELETE)，一个URL来确定要执行的操作，一组查询参数，以及用一个可选的多字段POST body来包含附加信息。这些属性的值依赖于你所发送的请求和目标服务提供者。参考所支持的社交网络站点文档，以获知可能的值。在表2-1中是文档的链接。

使用requestForServiceType:requestMethod:URL:parameters:函数，根据具体的参数，初始化一个新的请求对象。使用addMultipartData:withName:type:方法有选择性的指定一个多字段POST body。当创建好请求之后，使用performRequestWithHandler:方法发送请求，并指定一个handler，当请求完成的时候该handler会被调用。如果你已经有了一个发送机制，可以使用preparedURLRequest方法来创建一个请求，这样就可以通过NSURLConnection对象来发送请求。如果请求需要用户授权，那么给account属性设置一个ACAccount对象。

Social Services各自文档网站链接

| 网站 | 链接 |
| ---- | ---- |
| Facebook | https://developers.facebook.com/docs/ |
| Sina Weibo | http://open.weibo.com/wiki/ |
| Twitter | https://dev.twitter.com/docs |
| LinkedIn | https://developer.linkedin.com/rest |

#1 Initializing Requests

```swift
// 根据具体的参数，初始化一个新的请求对象。
public /*not inherited*/ init!(forServiceType serviceType: String!, requestMethod: SLRequestMethod, URL url: NSURL!, parameters: [NSObject : AnyObject]!)
```

#2 Accessing Properties

```swift
/// 用于授权请求的账号信息
public var account: ACAccount!
    
// 请求使用的方法
public var requestMethod: SLRequestMethod { get }
    
// 请求的目的UR
public var URL: NSURL! { get }
    
// 请求使用到的参数
public var parameters: [NSObject : AnyObject]! { get }
```

#3 Sending Requests

```swift
// 为请求指定多字段POST body
public func addMultipartData(data: NSData!, withName name: String!, type: String!, filename: String!)
    
// 返回一个授权URL请求，可以使用NSURLConnection对象来发送该请求
public func preparedURLRequest() -> NSURLRequest!
    
// 执行一个异步请求，并且当请求结束的时候调用指定的handler
public func performRequestWithHandler(handler: SLRequestHandler!)
```

&#160;

----------

#Appendix

##Related Documentation

[Social Framework Reference](https://developer.apple.com/library/ios/documentation/Social/Reference/Social_Framework/index.html)

[SLRequest Class Reference](https://developer.apple.com/library/ios/documentation/Social/Reference/SLRequest_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-25 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog