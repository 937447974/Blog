[URL Session(NSURLSession)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSession).md)

[URL Session(NSURLSessionDataTask))](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDataTask).md)

[URL Session(NSURLSessionUploadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionUploadTask).md)

[URL Session(NSURLSessionDownloadTask)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(NSURLSessionDownloadTask).md)

[URL Session(Cache).md](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(Cache).md)

[URL Session(Cookie)](https://github.com/937447974/Blog/blob/master/IOS/Core%20Services%20Layer/Foundation/URL%20Session(Cookie).md)

----

在这篇博文为大家讲解整个项目中最重要的功能：和服务器通信。相信现在的app单机操作的很少了，基本上都会和服务器通信。

# 1 NSURLSessionDataTask

和服务器通信使用到的类是NSURLSessionDataTask，早期的时候我们会使用NSURLConnection完成通信。

NSURLSessionDataTask是NSURLSession的子类，它向服务器发送数据的同时接受服务器发回的数据，是短连接操作。

# 2 搭建项目

你使用任何项目都没有关系，本次只是为了测试NSURLSessionDataTask的相关功能。为此使用类YJDataTaskVC。

```swift
//
//  YJDataTaskVC.swift
//  NSURLSession
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/3.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// NSURLSessionDataTask交互
class YJDataTaskVC: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
        
}
```

这是一个很干净的类，相信大家一目了然。你只要保证你的项目能进入这个VC即可。

# 3 异步请求

NSURLSession整个系列的请求都是高度异步的，当用户发出一个请求后，数据没回来的时候，还可以做其他操作。

网络请求时一般分为3步走。

1. 使用NSURLRequest或NSMutableURLRequest封装相关信息，如服务器地址、发送到服务器的信息、请求超时控制、请求方式等。
2. 使用NSMutableURLRequest创建不同的任务，并使用resume()方法将信息发送给服务器。
3. 在completionHandler闭包中处理服务器发回的数据。

## 3.1 Get请求

在通信中get是最简单的请求方式了，一个url链接即可获取服务器发回的数据。在YJDataTaskVC添加如下代码。

```swift
// MARK: - 异步
// MARK: Get请求
func sendRequestGet() {
    // 获取当前文件内容
    let urlStr = "https://raw.githubusercontent.com/937447974/Swift/master/NSURLSession/NSURLSession/YJDataTaskVC.swift"
    if let url = NSURL(string: urlStr) {
        // 包装请求地址和信息
        let request = NSURLRequest(URL: url)
        // 1. session处理，这里使用全局共享session
        let session = NSURLSession.sharedSession()
        // 2. 获取NSURLSessionDataTask
        let dataTask = session.dataTaskWithRequest(request) { (data: NSData?, response: NSURLResponse?, error: NSError?) -> Void in
            // 有回调数据
            if data != nil {
                self.jsonObjectWithData(data!)
            } else {
                print("发送请求出错:\(error?.localizedDescription)")
            }
        }
        dataTask.resume() // 3.发出http请求
    }
}

// MARK: - data数据转换
func jsonObjectWithData(data: NSData) -> [String: AnyObject]? {
    var dict: [String: AnyObject]? = nil
    do {
        // data -> AnyObject
        let jsonObject = try NSJSONSerialization.JSONObjectWithData(data, options: NSJSONReadingOptions.AllowFragments)
        dict = jsonObject as? [String: AnyObject]
        print("获取服务器的数据:\(dict)")
    } catch {
        print("服务器回调数据，转换出错:\(error)")
        print("原始数据：\(NSString(data: data, encoding: NSUTF8StringEncoding)))")
    }
    return dict
}
```

在方法sendRequestGet()中，我使用get获取github上当前类的源代码。这里的方法jsonObjectWithData是为了把服务器发回的NSData数据转换为字典。

在viewDidLoad()添加代码。

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // 异步
    self.sendRequestGet()
}
```

运行项目即可在控制台看见输出信息。

## 3.2 Post请求

在网络通信中，我们最常用的就是post提交数据了。如登录、注册、修改密码等功能都是通过异步Post完成的。

```swift
// MARK: - Post请求
func sendRequestPost() {
    let urlStr = "https://www.baidu.com"
    if let url = NSURL(string: urlStr) {
        // 包装请求地址和信息
        let request = NSMutableURLRequest(URL: url)
        request.HTTPMethod = "POST" //设置请求方式为POST，默认为GET
        request.addValue("text/xml", forHTTPHeaderField: "Content-Type")// 定义类型
        // 填充数据
        let dict = ["name": "阳君", "qq": "937447974"]
        do {
            try request.HTTPBody = NSJSONSerialization.dataWithJSONObject(dict, options: NSJSONWritingOptions.PrettyPrinted)
            print("发送数据:\(NSString(data: request.HTTPBody!, encoding: NSUTF8StringEncoding))")
        } catch {
            print("json处理出错:\(error)")
        }
        // 1. session处理，这里使用全局共享session
        let session = NSURLSession.sharedSession()
        // 2. 获取NSURLSessionDataTask
        let dataTask = session.dataTaskWithRequest(request) { (data: NSData?, response: NSURLResponse?, error: NSError?) -> Void in
            // 有回调数据
            if data != nil {
                self.jsonObjectWithData(data!)
            } else {
                print("发送请求出错:\(error?.localizedDescription)")
            }
        }
        dataTask.resume() // 3.发出http请求
    }
}
```

post请求要比get请求多几个步骤，多出的步骤就是将我们的数据封装到NSMutableURLRequest。

# 4 同步

在NSURLSession中是没有同步的，但是有的时候因为业务需要，用户的请求需要同步操作。这里我们可以使用NSURLConnection完成同步操作。

这里只演示Post的同步请求。关于Get，和NSURLSession的get请求一样，这里不再详细描述。

```swift
// MARK: - 同步Post请求
func sendSynchronousRequestPOST() {
    let urlStr = "https://www.baidu.com"
    if let url = NSURL(string: urlStr) {
        // 包装请求地址和信息
        let request = NSMutableURLRequest(URL: url)
        request.HTTPMethod = "POST" //设置请求方式为POST，默认为GET
        request.addValue("text/xml", forHTTPHeaderField: "Content-Type")// 定义类型
        // 填充数据
        let dict = ["name": "阳君", "qq": "937447974"]
        do {
            try request.HTTPBody = NSJSONSerialization.dataWithJSONObject(dict, options: NSJSONWritingOptions.PrettyPrinted)
            print("发送数据:\(NSString(data: request.HTTPBody!, encoding: NSUTF8StringEncoding))")
        } catch {
            print("json处理出错:\(error)")
        }
        // 同步使用
        do {
            let data = try NSURLConnection.sendSynchronousRequest(request, returningResponse: nil)
            self.jsonObjectWithData(data)
        } catch {
            print("发送请求出错:\(error)")
        }
        
    }
}
```

相关测试可自行完成。
&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[URL Session Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-04 | 博文完成 |
| 2015-12-12 | 更改链接 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog