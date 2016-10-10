WCSession主要用于促进iOS和watchkit中的app交流信息，可快速立刻传递信息。当一边处于后台状态时，另一端传输数据，此时所有业务会在后台执行。

##Configuring and Activating the Session

创建和启动session可使用如下代码

```swift
if WCSession.isSupported() {
    let session = WCSession.default()
    session.delegate = self
    session.activate()
}
```

然后通过delegate获取相关信息。

##Supporting Communication with Multiple Apple Watches

当用户切换watch时会发生如下生命周期变化。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016100901.png)

------

#Symbols

##1 Getting the Default Session

```swift
// 是否支持
open class func isSupported() -> Bool
// 获取默认session
open class func `default`() -> WCSession
```

##2 Configuring the Session

```swift
// 代理
weak open var delegate: WCSessionDelegate?
// 激活session
open func activate()
// 获取session状态
@available(iOS 9.3, *)
open var activationState: WCSessionActivationState { get }
```

##3 Getting the Paired Device Information


```swift
// 设备是否配对watch
open var isPaired: Bool { get }
// Watch app是否安装
open var isWatchAppInstalled: Bool { get }
// 并发是否可用
open var isComplicationEnabled: Bool { get }
// 存放配对信息的地址
open var watchDirectoryURL: URL? { get }
```

##4 Determining the Session’s Reachability

```swift
// 能否传递信息
open var isReachable: Bool { get }
```

##5 Managing Background Updates

```swift
// 获取信息
open var applicationContext: [String : Any] { get }
// 更新信息
open func updateApplicationContext(_ applicationContext: [String : Any]) throws
// 获取最后更新的信息
open var receivedApplicationContext: [String : Any] { get }
```

##6 Sending Messages

```swift
// 发送[String : Any]信息
open func sendMessage(_ message: [String : Any], replyHandler: (@escaping ([String : Any]) -> Swift.Void)?, errorHandler: (@escaping (Error) -> Swift.Void)? = nil)
// 发送data信息
open func sendMessageData(_ data: Data, replyHandler: (@escaping (Data) -> Swift.Void)?, errorHandler: (@escaping (Error) -> Swift.Void)? = nil)
```

##7 Updating Complication Data

```swift
// 剩下的未发送的并发数
@available(iOS 10.0, *)
open var remainingComplicationUserInfoTransfers: Int { get }
// 并发传输信息
open func transferCurrentComplicationUserInfo(_ userInfo: [String : Any] = [:]) -> WCSessionUserInfoTransfer
```

##8 Transferring Data in the Background

```swift
// 传输信息
open func transferUserInfo(_ userInfo: [String : Any] = [:]) -> WCSessionUserInfoTransfer
// 正在船速的WCSessionUserInfoTransfer数组
open var outstandingUserInfoTransfers: [WCSessionUserInfoTransfer] { get }
```

##9 Transferring Files in the Background

```swift
// 是否有未发送的数据
@available(iOS 10.0, *)
open var hasContentPending: Bool { get }
// 发送文件
open func transferFile(_ file: URL, metadata: [String : Any]?) -> WCSessionFileTransfer
// 获取正在发送的文件
open var outstandingFileTransfers: [WCSessionFileTransfer] { get }
```

&#160;

----------

#Appendix

##Related Documentation

[WCSession](https://developer.apple.com/reference/watchconnectivity/wcsession)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-09 | 博文完成 |

##Copyright

CSDN：[http://blog.csdn.net/y550918116j](http://blog.csdn.net/y550918116j)

GitHub：[https://github.com/937447974](https://github.com/937447974)