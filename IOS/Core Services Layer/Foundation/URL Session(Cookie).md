[URL Session(NSURLSession)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSession).md)

[URL Session(NSURLSessionDataTask))](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDataTask).md)

[URL Session(NSURLSessionUploadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionUploadTask).md)

[URL Session(NSURLSessionDownloadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDownloadTask).md)

[URL Session(Cache).md](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(Cache).md)

[URL Session(Cookie)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(Cookie).md)

----

当你访问一个网站时，NSURLRequest都会帮你主动记录下来你访问的站点设置的cookie，如果Cookie存在的话，会把这些信息放在NSHTTPCookieStorage容器中共享，当你下次再访问这个站点时，NSURLRequest会拿着上次保存下来了的cookie继续去请求。

cookie和缓存一样有两个类控制。

1. NSHTTPCookieStorage：Cookie管理器。
2. NSHTTPCookie：Cookie实际对象。

#1 NSHTTPCookieStorage

NSHTTPCookieStorage实际是一个共享的单例对象，它存储整个应用的所有Cookie，并在所有线程中是同步的。

##1.1 获取NSHTTPCookieStorage

```swift
// 获取共享的NSHTTPCookieStorage
public class func sharedHTTPCookieStorage() -> NSHTTPCookieStorage
// 获取应用程序组的NSHTTPCookieStorage
public class func sharedCookieStorageForGroupContainerIdentifier(identifier: String) -> NSHTTPCookieStorage
```

##1.2 获取和设置Cookie访问策略

```swift
public var cookieAcceptPolicy: NSHTTPCookieAcceptPolicy

public enum NSHTTPCookieAcceptPolicy : UInt {
    case Always // 全部允许
    case Never // 全部不允许
    case OnlyFromMainDocumentDomain // 只允许顶级地址的cookie通过
}
```

##1.3 增加和删除Cookie

```swift
// 增加cookie
public func setCookie(cookie: NSHTTPCookie)
// 增加cookie的同时绑定地址
public func setCookies(cookies: [NSHTTPCookie], forURL URL: NSURL?, mainDocumentURL: NSURL?)
// 删除指定cookie
public func deleteCookie(cookie: NSHTTPCookie)
// 根据日期删除cookie
public func removeCookiesSinceDate(date: NSDate)
```

##1.4 获取Cookie

```swift
// 获取所有cookie
public var cookies: [NSHTTPCookie]? { get }
// 根据路径放回cookie
public func cookiesForURL(URL: NSURL) -> [NSHTTPCookie]?
// 获取所有cookie并排序
public func sortedCookiesUsingDescriptors(sortOrder: [NSSortDescriptor]) -> [NSHTTPCookie]
```

##1.5 通知

```swift
// cookieAcceptPolicy变动通知
public let NSHTTPCookieManagerAcceptPolicyChangedNotification: String
// cookie 变化
public let NSHTTPCookieManagerCookiesChangedNotification: String
```

#2 NSHTTPCookie

NSHTTPCookie是cookie的实际对象。这里不再详细描述，有兴趣的朋友查阅API《[NSHTTPCookie Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSHTTPCookie_Class/index.html)》

#3 实战演练

```swift
//
//  YJHTTPCookieVC.swift
//  NSURLSession
//
//  Created by yangjun on 15/12/5.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// NSHTTPCookie
class YJHTTPCookieVC: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 共享cookie
        let sharedHTTPCookie = NSHTTPCookieStorage.sharedHTTPCookieStorage()
        
        // 手动创建一个cookie
        var dict = [String : AnyObject]()
        dict[NSHTTPCookieName] = "阳君"
        dict[NSHTTPCookieValue] = "937447974"
        dict[NSHTTPCookieVersion] = 1
        dict[NSHTTPCookieDomain] = "blog.csdn.net"
        dict[NSHTTPCookiePath] = "/"
        if let cookie = NSHTTPCookie(properties: dict) {
            print("手动创建\(cookie.properties)")
            sharedHTTPCookie.setCookie(cookie)
        }
        
        // 删除所有
        if let list = sharedHTTPCookie.cookies {
            // 获取cookie的header
            print(NSHTTPCookie.requestHeaderFieldsWithCookies(list))
            for cookie in list {
                // 读取cookie
                print(cookie.properties)
                // 删除cookie
                sharedHTTPCookie.deleteCookie(cookie)
            }
        }
    }

}
```

&#160;

----------

#其他

##参考资料

[URL Session Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)

[NSHTTPCookie Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSHTTPCookie_Class/index.html)

[NSHTTPCookie Class Reference]

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-05 | 博文完成 |
| 2015-12-12 | 更改链接 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog