[URL Session(NSURLSession)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSession).md)

[URL Session(NSURLSessionDataTask))](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDataTask).md)

[URL Session(NSURLSessionUploadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionUploadTask).md)

[URL Session(NSURLSessionDownloadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDownloadTask).md)

[URL Session(Cache).md](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(Cache).md)

[URL Session(NSURLSessionDownloadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(Cookie).md)

----

在上一篇博文《[URL Session(NSURLSessionUploadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionUploadTask).md)》为大家讲解了上传文件，在这篇博文为大家带来下载功能，并且支持并发下载。

下载文件使用的类是NSURLSessionDownloadTask。

#1 NSURLSessionDownloadTask

NSURLSessionDownloadTask是NSURLSessionTask的子类，主要处理网络中的下载业务。

下载下来的文件会存储到一个临时文件中，当应用退出后会被销毁。如果你想保存这些数据，需要你将这些数据移动到持久化目录，如document。

在NSURLSessionTask中有以下三个方法。

```swift
// 开始任务
public func resume()
// 挂起任务
public func suspend()
// 取消任务
public func cancel()
```

前面使用了resume()方法，今天会使用suspend()和cancel()为大家带来暂停下载和取消下载的功能。

#2 NSURLSessionDownloadDelegate

在下载的时候，绝大多数的情况都是使用后台下载的。因为用户不可能长时间在一个页面等待下载。后台下载的监听就需要NSURLSessionDownloadDelegate实现了。在NSURLSessionDownloadDelegate中有如下几个方法。

```swift
// MARK: 下载完成
public func URLSession(session: NSURLSession, downloadTask: NSURLSessionDownloadTask, didFinishDownloadingToURL location: NSURL)

// MARK: 下载中(会多次调用，可以记录下载进度) 
optional public func URLSession(session: NSURLSession, downloadTask: NSURLSessionDownloadTask, didWriteData bytesWritten: Int64, totalBytesWritten: Int64, totalBytesExpectedToWrite: Int64)

// MARK: 重新开始下载
optional public func URLSession(session: NSURLSession, downloadTask: NSURLSessionDownloadTask, didResumeAtOffset fileOffset: Int64, expectedTotalBytes: Int64)
```

#3 后台下载

##3.1 YJDownloadTaskVC类

下载测试我们使用YJDownloadTaskVC类。

```swift
//
//  YJDownloadTaskVC.swift
//  NSURLSession
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/3.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

// NSURLSessionDownloadTask 下载
class YJDownloadTaskVC: UIViewController, NSURLSessionDownloadDelegate {

    /// 下载器
    private var downloadTask: NSURLSessionDownloadTask?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // 相关按钮
        // 刷新
        let refreshItem = UIBarButtonItem(barButtonSystemItem: UIBarButtonSystemItem.Refresh, target: self, action: "downloadTaskRefresh")
        // 开始
        let resumeItem = UIBarButtonItem(barButtonSystemItem: UIBarButtonSystemItem.Play, target: self, action: "downloadTaskResume")
        self.navigationItem.leftBarButtonItems = [refreshItem, resumeItem]
        // 暂停
        let suspendItem = UIBarButtonItem(barButtonSystemItem: UIBarButtonSystemItem.Pause, target: self, action: "downloadTaskSuspend")
        // 取消
        let cancelItem = UIBarButtonItem(barButtonSystemItem: UIBarButtonSystemItem.Cancel, target: self, action: "downloadTaskCancel")
        self.navigationItem.rightBarButtonItems = [cancelItem, suspendItem]
    
    }
    
    // MARK: - action
    // MARK: 刷新
    func downloadTaskRefresh() {

    }
    
    // MARK: 开始下载
    func downloadTaskResume() {
    }
    
    // MARK: 暂停下载
    func downloadTaskSuspend() {
    }
    
    // MARK: 取消下载
    func downloadTaskCancel() {
    }
    
}
```

这里已经创建了4个按钮。刷新、开始下载、暂停下载和取消下载，并且创建了一个全局属性downloadTask指向一个NSURLSessionDownloadTask。

##3.2 创建NSURLSession

在这里我们会以单例的模式创建NSURLSession，这样可以在调度器里面并发请求服务器。

```swift
// MARK: - 后台下载
// MARK: 获取后台下载的session
private func backgroundSession() -> NSURLSession{
    struct Static {
        static var onceToken: dispatch_once_t = 0
        static var session: NSURLSession!
    }
    //var static session: NSURLSession!
    dispatch_once(&Static.onceToken, { () -> Void in
        let sessionConfiguration = NSURLSessionConfiguration.backgroundSessionConfigurationWithIdentifier("com.downloadTask.URLSession")
        sessionConfiguration.timeoutIntervalForRequest = 20// 请求超时时间
        sessionConfiguration.discretionary = true // 系统自动选择最佳网络下载
        sessionConfiguration.HTTPMaximumConnectionsPerHost = 5// 限制每次最多5个连接
        Static.session = NSURLSession(configuration: sessionConfiguration, delegate: self, delegateQueue: nil)//指定配置和代理
    })
    return Static.session
}
```

##3.3 NSURLSessionDelegate代理实现

通过NSURLSessionDelegate代理，我们能监听下载的进度，以及下载后的链接和下载中的错误。

```swift
// MARK: - NSURLSessionDelegate
// MARK: 下载完成
func URLSession(session: NSURLSession, downloadTask: NSURLSessionDownloadTask, didFinishDownloadingToURL location: NSURL) {
    // 下载完成的地址
    print(location)
}
    
// MARK: 下载中(会多次调用，可以记录下载进度)
func URLSession(session: NSURLSession, downloadTask: NSURLSessionDownloadTask, didWriteData bytesWritten: Int64, totalBytesWritten: Int64, totalBytesExpectedToWrite: Int64) {
    let progress: Float = Float(totalBytesWritten) / Float(totalBytesExpectedToWrite)
    print("\(totalBytesExpectedToWrite)--\(totalBytesWritten)--\(progress)")
}
    
// MARK: 重新开始下载
func URLSession(session: NSURLSession, downloadTask: NSURLSessionDownloadTask, didResumeAtOffset fileOffset: Int64, expectedTotalBytes: Int64) {
    let progress: Float = Float(fileOffset) / Float(expectedTotalBytes)
    print("已下载\(progress)")
}
    
// MARK: - NSURLSessionTaskDelegate
// MARK: 任务完成，不管是否下载成功
func URLSession(session: NSURLSession, task: NSURLSessionTask, didCompleteWithError error: NSError?) {
    print("错误: \(error)")
}
```

为了监听下载中的错误，我们实现了NSURLSessionTaskDelegate的`func URLSession(session: NSURLSession, task: NSURLSessionTask, didCompleteWithError error: NSError?)`方法。不管成功还是失败，任务结束后都会调用它。

##3.4 业务实现

###3.4.1 并发测试

这里是从网上下载一个高清图片，并在队列中连续下载10张照片，由于我们session中设置了并发数为5个，你会发现控制台输出只会同时下载5张照片。

```swift
// MARK: - 队列测试
func testQueue() {
    let usrString = "http://g.hiphotos.baidu.com/image/pic/item/472309f790529822c4ac8ad0d5ca7bcb0a46d402.jpg"
    let url = NSURL(string: usrString)
    let request = NSMutableURLRequest(URL: url!)
    let session = self.backgroundSession()
    // 并发下载10个文件
    for _ in 0..<10 {
        session.downloadTaskWithRequest(request).resume()
    }
}
```

###3.4.2 刷新

刷新的实质是设置downloadTask。

```swift
// MARK: - action
// MARK: 刷新
func downloadTaskRefresh() {
    // 这是一张高清图片
    let usrString = "http://g.hiphotos.baidu.com/image/pic/item/472309f790529822c4ac8ad0d5ca7bcb0a46d402.jpg"
    let url = NSURL(string: usrString)
    let request = NSMutableURLRequest(URL: url!)
    let session = self.backgroundSession()
    self.downloadTask = session.downloadTaskWithRequest(request)
}
```

###3.4.3 开始下载

```swift
// MARK: 开始下载
func downloadTaskResume() {
    self.downloadTask?.resume()
}
```
###3.4.4 暂停下载

```swift
// MARK: 暂停下载
func downloadTaskSuspend() {
    self.downloadTask?.suspend()
}
```

###3.4.5 取消下载

```swift
// MARK: 取消下载
func downloadTaskCancel() {
    self.downloadTask?.cancel()
}
```

到这里整个项目就结束了，你可以运行项目，点击相关按钮查看下载的效果。
&#160;

----------

#其他

##参考资料

[URL Session Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)

[NSURLSessionDownloadTask Class Reference](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSURLSessionDownloadTask_class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-05 | 博文完成 |
| 2015-12-12 | 更改链接 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog