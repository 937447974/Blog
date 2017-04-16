[URL Session(NSURLSession)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSession).md)

[URL Session(NSURLSessionDataTask))](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDataTask).md)

[URL Session(NSURLSessionUploadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionUploadTask).md)

[URL Session(NSURLSessionDownloadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDownloadTask).md)

[URL Session(Cache).md](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(Cache).md)

[URL Session(Cookie)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(Cookie).md)

----

在上一篇博文《[URL Session(NSURLSessionDownloadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDownloadTask).md)》为大家讲解了后台下载文件的功能，这篇博文将向大家介绍NSURLSession系列的缓存。这是一个非常强大的功能，如webView加载太慢，我们就可以缓存网页以达到页面显示加速的效果。

缓存有两个核心类：

1. NSURLCache：缓存管理器。
2. NSCachedURLResponse：存储缓存的对象。

# 1 NSURLCache

NSURLCache缓存控制器，多数情况下我们只需要操作它。

## 1.1 获取和设置共享缓存

我们可以使用系统配置的缓存，也可以设计自己的缓存管理器。

```swift
// 获取共享缓存
public class func sharedURLCache() -> NSURLCache
// 设置共享缓存
public class func setSharedURLCache(cache: NSURLCache)
```

## 1.2 创建共享缓存

当你想设置整个系统的共享缓存时，首先要创建一个NSURLCache。

```swift
/// 创建缓存
///
/// - parameter memoryCapacity : 内存占用空间，单位字节
/// - parameter diskCapacity : 硬盘占用空间，单位字节
/// - parameter diskPath : 硬盘存储路径
///
/// - returns: NSURLCache
public init(memoryCapacity: Int, diskCapacity: Int, diskPath path: String?)
```

## 1.3 获取和存储缓存对象

有的时候你也可以自己去获取和存储缓存对象。更多的时候我们是获取NSCachedURLResponse，由系统去自动存储缓存NSCachedURLResponse。

```swift
// 获取缓存对象
public func cachedResponseForRequest(request: NSURLRequest) -> NSCachedURLResponse?
// 存储缓存对象
public func storeCachedResponse(cachedResponse: NSCachedURLResponse, forRequest request: NSURLRequest)
```

## 1.4 删除缓存对象

由于业务需要，我们也需要删除缓存对象。

```swift
// 删除所有缓存对象
public func removeAllCachedResponses()
// 根据url删除指定缓存对象
public func removeCachedResponseForRequest(request: NSURLRequest)
// 根据日期删除缓存对象
public func removeCachedResponsesSinceDate(date: NSDate)
```

## 1.5 获取和设置磁盘缓存属性

我们可以在开发工程中设置和获取磁盘缓存属性。

```swift
// 缓存最大可占用磁盘空间
public var diskCapacity: Int
// 缓存已占用磁盘空间
public var currentDiskUsage: Int { get }
```

## 1.6 获取和设置内存缓存属性

还可以获取和设置内存缓存属性。

```swift
// 缓存最大可占用内存空间
public var memoryCapacity: Int
// 缓存已占用内存空间
public var currentMemoryUsage: Int { get }
```

# 2 NSCachedURLResponse

NSCachedURLResponse就是我们实际缓存的每一个对象，一般不对其操作，这里不在详细说明。

# 3 缓存模式

还记得前面讲的NSURLSession的两步，封装网络请求相关信息和根据不同工作模式发出请求。这里的缓存配置是在NSMutableURLRequest中配置的。

在NSMutableURLRequest中有个属性cachePolicy，这个就是缓存模式，指向一个NSURLRequestCachePolicy枚举对象。

```swift
NSURLRequestCachePolicy : UInt {
    case UseProtocolCachePolicy // 默认的缓存策略（取决于协议)
    case ReloadIgnoringLocalCacheData // 忽略缓存直接从原始地址下载
    case ReloadIgnoringLocalAndRemoteCacheData // 未实现
    case ReturnCacheDataElseLoad // 只有在cache中不存在data时,才从原始地址下载
    case ReturnCacheDataDontLoad // 只使用cache数据，如果不存在cache，请求失败;用于没有建立网络连接离线模式;
    case ReloadRevalidatingCacheData // 未实现
}
```

# 4 实战演练

接下来，我们就用一个webView请求的方法测试缓存。第二次请求使用缓存数据，已达到缓存加速的效果。

## 4.1 显示网页

```swift
//
//  YJURLCacheVC.swift
//  NSURLSession
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/5.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit
import WebKit

/// 缓存
class YJURLCacheVC: UIViewController {
    
    /// WKWebView
    private var webView: WKWebView!

    override func viewDidLoad() {
        super.viewDidLoad()
        // 刷新按钮
        self.navigationItem.rightBarButtonItem = UIBarButtonItem(barButtonSystemItem: UIBarButtonSystemItem.Refresh, target: self, action: "reloadWebView")
        // 初始化 WKWebView
        self.webView = WKWebView(frame: self.view.frame)
        self.view.addSubview(self.webView)
    }

    // MARK: - 刷新
    func reloadWebView() {
//        let url = NSURL(string: "http://g.hiphotos.baidu.com/image/pic/item/472309f790529822c4ac8ad0d5ca7bcb0a46d402.jpg")!
        let url = NSURL(string: "http://blog.csdn.net/y550918116j")!
        let request = NSMutableURLRequest(URL: url)
        self.webView.loadRequest(request)
    }

}
```

这是一个简单的页面显示，使用WKWebView显示http://blog.csdn.net/y550918116j。你会发现每次点击刷新按钮，加载的速度不是特别快。

## 4.2 缓存显示网页

接下来就使用缓存，加速显示页面。改写reloadWebView()方法。

```swift
// MARK: - 刷新
func reloadWebView() {
    let urlCache = NSURLCache.sharedURLCache()
    var str = "memoryCapacity:\(urlCache.memoryCapacity)"
    str += "; diskCapacity:\(urlCache.diskCapacity)"
    str += "; currentMemoryUsage:\(urlCache.currentMemoryUsage)"
    str += "; currentDiskUsage:\(urlCache.currentDiskUsage)"
    print(str)
    
    // 缓存显示照片
//        let url = NSURL(string: "http://g.hiphotos.baidu.com/image/pic/item/472309f790529822c4ac8ad0d5ca7bcb0a46d402.jpg")!
    let url = NSURL(string: "http://blog.csdn.net/y550918116j")!
    let request = NSMutableURLRequest(URL: url)
    
    // 缓存成功后，开启本地缓存
    if NSURLCache.sharedURLCache().cachedResponseForRequest(request) != nil {
        /* 缓存策略
        NSURLRequestCachePolicy : UInt {
        case UseProtocolCachePolicy // 默认的缓存策略（取决于协议)
        case ReloadIgnoringLocalCacheData // 忽略缓存直接从原始地址下载
        case ReloadIgnoringLocalAndRemoteCacheData // 未实现
        case ReturnCacheDataElseLoad // 只有在cache中不存在data时,才从原始地址下载
        case ReturnCacheDataDontLoad // 只使用cache数据，如果不存在cache，请求失败;用于没有建立网络连接离线模式;
        case ReloadRevalidatingCacheData // 未实现
        }
        */
        request.cachePolicy = NSURLRequestCachePolicy.ReturnCacheDataDontLoad // 提取缓存数据
    } else {
        urlCache.removeAllCachedResponses() // 清楚所有缓存
        urlCache.removeCachedResponseForRequest(request) // 根据地址清楚缓存
        urlCache.diskCapacity = 10*1024*1024 // 磁盘缓存，10M，单位字节
        urlCache.memoryCapacity = 1*1024*1024 // 内存缓存，1M，单位字节
        // 发出请求才会缓存数据
        NSURLSession.sharedSession().dataTaskWithRequest(request).resume()
    }
    self.webView.loadRequest(request)
}
```

这里可以自行测试缓存网页和照片。会发现网页缓存到内存，照片缓存到磁盘中了。


&#160;

----------

# 其他

## 参考资料

[URL Session Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)

[NSURLCache Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLCache_Class/index.html)

[NSCachedURLResponse Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSCachedURLResponse_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-05 | 博文完成 |
| 2015-12-12 | 更改链接 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog