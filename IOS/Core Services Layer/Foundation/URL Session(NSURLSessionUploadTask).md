[URL Session(NSURLSession)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSession).md)

[URL Session(NSURLSessionDataTask))](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDataTask).md)

[URL Session(NSURLSessionUploadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionUploadTask).md)

[URL Session(NSURLSessionDownloadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDownloadTask).md)

[URL Session(Cache).md](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(Cache).md)

[URL Session(NSURLSessionDownloadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(Cookie).md)

----

在上一篇博文《[URL Session(NSURLSessionDataTask))](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDataTask).md)》中讲解了怎么和服务器通信，在这篇博文向大家讲解怎么把文件上传到服务器。

#1 NSURLSessionUploadTask

上传文件使用NSURLSessionUploadTask来操作。通过NSURLSession创建NSURLSessionUploadTask有如下几种方式。

1. `uploadTaskWithRequest(_:fromData:)`：上传NSData数据，只能前台上传；
2. `uploadTaskWithRequest(_:fromData:completionHandler:)`：上传NSData数据，完成后回调，只能前台上传；
3. `uploadTaskWithRequest(_:fromFile:)`：通过文件路径上传文件，可前后台上传；
4. `uploadTaskWithRequest(_:fromFile:completionHandler:)`：通过文件路径上传文件，完成后回调，可前后台上传；
5. `uploadTaskWithStreamedRequest(_:)`：通过流的方式上传数据。

#2 前台上传

前台上传的时候都是上传的小数据，大数据多数放在后台上传。毕竟上传是需要时间的，等待太久用户体验不好。

```swift
// MARK: 前台上传
func uploadTaskMain() {
    // url
    let urlStr = "https://www.baidu.com"
    let url = NSURL(string: urlStr)
    if url == nil {
        return
    }
    // NSURLRequest配置
    let request = NSMutableURLRequest(URL: url!)
    request.HTTPMethod = "POST" //设置请求方式为POST，默认为GET
    // 需要添加相关header
    // request.addValue("text/xml", forHTTPHeaderField: "Content-Type")// 定义类型
    // request.addValue("0", forHTTPHeaderField: "Content-Length") //
    // session
    let session = NSURLSession.sharedSession()
    let completionHandler = {(data:NSData?, response:NSURLResponse?, error:NSError?) -> Void in
        if error == nil {
            print("上传成功")
        } else {
            print(error)
        }
    }
    // NSURLSessionUploadTask
    let uploadTask = session.uploadTaskWithRequest(request, fromData: nil, completionHandler: completionHandler)
    uploadTask.resume()//发出请求
}
```

#3 后台上传

很多时候我们会在后台上传数据，尤其是特别大的数据，还会考虑分段上传。

分段上传其实不是很难，讲个简单的思路，如我们将一个文件分成N个文件，一个NSdata数据分成N个数据，服务器接受到所有数据后，按照一定的顺序组装数据并保存。

```swift
// MARK: 后台上传
func uploadTaskBackground() {
    // url
    let urlStr = "https://www.baidu.com"
    let url = NSURL(string: urlStr)
    if url == nil {
        return
    }
    // NSURLRequest配置
    let request = NSMutableURLRequest(URL: url!)
    request.HTTPMethod = "POST" //设置请求方式为POST，默认为GET
    // 需要添加相关header
    // request.addValue("text/xml", forHTTPHeaderField: "Content-Type")// 定义类型
    // request.addValue("0", forHTTPHeaderField: "Content-Length") //
    // 会话配置
    let configuration = NSURLSessionConfiguration.backgroundSessionConfigurationWithIdentifier("com.uploadTask.URLSession")
    configuration.HTTPMaximumConnectionsPerHost = 5// 最多同时上传5个文件
    configuration.discretionary = true // 系统自动选择最佳网络
    configuration.timeoutIntervalForRequest = 20 // 请求超时时间
    // 会话
    let session = NSURLSession(configuration: configuration)
    // NSURLSessionUploadTask
    let dataFile = NSBundle.mainBundle().URLForResource("Info", withExtension: "plist")
    let uploadTask = session.uploadTaskWithRequest(request, fromFile: dataFile!)
    // 发出请求
    uploadTask.resume()
}
```

后台上传相对于前台上传要复杂一点。会给上传的数据打上标签Identifier。当上传完成后，我们就可以在AppDelegate.swift中收到通知。通知回调的方法是：

```swift
// MARK: 后台任务
func application(application: UIApplication, handleEventsForBackgroundURLSession identifier: String, completionHandler: () -> Void) {
    print("handleEventsForBackgroundURLSession:\(identifier)")
}
```

&#160;

----------

#其他

##参考资料

[NSURLSessionUploadTask](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSURLSessionUploadTask_class/index.html)

[URL Session Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)

[NSURLSession Class Reference](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSURLSession_class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-04 | 博文完成 |
| 2015-12-12 | 更改链接 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog