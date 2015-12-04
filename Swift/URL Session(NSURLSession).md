[URL Session(NSURLSession)](https://github.com/937447974/Blog/blob/master/Swift/URL%20Session(NSURLSession).md)

[URL Session(NSURLSessionDataTask))](https://github.com/937447974/Blog/blob/master/Swift/URL%20Session(NSURLSessionDataTask).md)

[URL Session(NSURLSessionUploadTask)](https://github.com/937447974/Blog/blob/master/Swift/URL%20Session(NSURLSessionUploadTask).md)

[URL Session(NSURLSessionDownloadTask)](https://github.com/937447974/Blog/blob/master/Swift/URL%20Session(NSURLSessionDownloadTask).md)

[URL Session(Cache)](https://github.com/937447974/Blog/blob/master/Swift/URL%20Session(Cache).md)

[URL Session(Cookie)](https://github.com/937447974/Blog/blob/master/Swift/URL%20Session(Cookie).md)

----

在2013年，苹果公司发布了关于NSURLSession的框架，这个框架是随着IOS7一起推出的。它和IOS7以前推出的NSURLConnection是一样的作用，可以理解为NSURLConnection的升级版。它能帮助我们更细腻化的监听网络交互的各种状态。

NSURLSession的相关类是在Foundation框架中，你可以使用它们完成一系列的网络交互。

#1 NSURLSession

NSURLSession在网络交互中是核心类，一切的工作都是在它的基础之上完成的。

##1.1 访问的数据类型

通过NSURLSession可以访问如下类型的数据。

1. ftp://
2. http://
3. https://
4. file:///
5. data://

##1.2 工作模式

NSURLSession基于不同的业务会使用不同的工作模式，这些工作模式定义不同的连接行为，如并发访问服务器、是否允许通过手机网络连接等等。考虑到这些业务，NSURLSession有四种工作模式：

1. 单例共享模式（sharedSession）：它是一个全局的单例，当我们的业务不是特别复杂时，就可以使用这种模式。
1. 默认会话模式（default）：工作模式类似于原来的NSURLConnection，使用的是基于磁盘缓存的持久化策略，使用用户keychain中保存的证书进行认证授权。
2. 瞬时会话模式（ephemeral）：该模式不使用磁盘保存任何数据。所有和会话相关的caches，证书，cookies等都被保存在RAM中，因此当程序使会话无效，这些缓存的数据就会被自动清空。
3. 后台会话模式（background）：该模式在后台完成上传和下载，在创建Configuration对象的时候需要提供一个NSString类型的ID用于标识完成工作的后台会话。

##1.3 任务类型

对于一个会话，你可以使用不同的工作模式和服务器完成交互。对于这些交互我们分为三种任务类型：

1. 加载数据：使用NSURLSessionDataTask和服务器完成交互，如登陆发送账号密码，服务器返回是否成功的状态。
2. 下载：使用NSURLSessionDownloadTask将服务器的文件下载到本地。
3. 上传：使用NSURLSessionUploadTask向服务器上传文件。

##1.4 异步交互

和大多数的网络API一样，NSURLSession是高度异步的。它将数据返回到我们的APP使用了下面两种方式。

1. 当传输成功或失败时，以块的方式处理返回的数据，它只在请求完成后调用。
2. 当数据传输或接受时，以代理的方式处理相关信息。

#2 线程安全

使用NSURLSession相关API在线程中是很安全的，我们可以在任何线程、队列、前台或后台使用它。

#3 层次结构

关于整个系统主要分为五大类，如图所示。

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015120301.png)

##3.1 核心类

关于NSURLSession的层次机构包含以下类和关系（嵌套的为子类关系）。

- NSURLSession：会话对象。
- NSURLSessionConfiguration：创建会话对象的相关配置。
- NSURLSessionTask：会话任务的基类。
    - NSURLSessionDataTask：网络通信，向服务器发送数据的同时，接受服务器反馈的信息。
        - NSURLSessionUploadTask：向服务器上传文件。
    - NSURLSessionDownloadTask：从服务器下载文件。

##3.2 协议

在NSURLSession相关API中也有很多协议，它能帮助我们更细腻的监听NSURLSession的工作状态。

- NSURLSessionDelegate：会话层次的协议。
- NSURLSessionTaskDelegate：任务层次的协议。
- NSURLSessionDataDelegate：监听NSURLSessionDataTask和NSURLSessionUploadTask的工作状态。
- NSURLSessionDownloadDelegate：监听NSURLSessionDownloadTask的工作状态。

##3.3 NSURL

要让NSURLSession运作起来，需要管理网络地址对象NSURL。下面是NSURL的关系图。

- NSURL：包含一个URL地址。
- NSURLRequest：包含NSURL、相关请求方法和属性。
- NSURLResponse：相关元数据的响应请求,如获取MIME类型和长度等内容。
    - NSHTTPURLResponse：特定的元数据，如请求header。
- NSCachedURLResponse：封装NSURLResponse对象，主要用于缓存数据。

&#160;

----------

#其他

##参考资料

[NSURLSession](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSURLSession_class/index.html)

[URL Session Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-03 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog