WCSessionDelegate用户响应WCSession相关操作，如传输data、file。

# Symbols

## 1 Managing Session Activation

```swift
// session激活
@available(iOS 9.3, *)
public func session(_ session: WCSession, activationDidCompleteWith activationState: WCSessionActivationState, error: Error?)
// 关闭session
@available(iOS 9.3, *)
public func sessionDidBecomeInactive(_ session: WCSession)
// 数据交互完毕
@available(iOS 9.3, *)
public func sessionDidDeactivate(_ session: WCSession)
```

## 2 Managing State Changes

```swift
// 状态发生变化
@available(iOS 9.0, *)
optional public func sessionWatchStateDidChange(_ session: WCSession)
// 连通性发生变化
@available(iOS 9.0, *)
optional public func sessionReachabilityDidChange(_ session: WCSession)
```

## 3 Receiving Context Data

```swift
// 接收上下文信息
@available(iOS 9.0, *)
optional public func session(_ session: WCSession, didReceiveApplicationContext applicationContext: [String : Any])
```

## 4 Receiving Immediate Messages

```swift
// 接收[String : Any]
@available(iOS 9.0, *)
optional public func session(_ session: WCSession, didReceiveMessage message: [String : Any])
@available(iOS 9.0, *)
optional public func session(_ session: WCSession, didReceiveMessage message: [String : Any], replyHandler: @escaping ([String : Any]) -> Swift.Void)

// 接收Data
@available(iOS 9.0, *)
optional public func session(_ session: WCSession, didReceiveMessageData messageData: Data)
@available(iOS 9.0, *)
optional public func session(_ session: WCSession, didReceiveMessageData messageData: Data, replyHandler: @escaping (Data) -> Swift.Void)
```

## 5 Managing Data Dictionary Transfers

```swift
// 接收WCSessionUserInfoTransfer
@available(iOS 9.0, *)
optional public func session(_ session: WCSession, didFinish userInfoTransfer: WCSessionUserInfoTransfer, error: Error?)
    
// 接收 userInfo: [String : Any]
@available(iOS 9.0, *)
optional public func session(_ session: WCSession, didReceiveUserInfo userInfo: [String : Any] = [:])
```

## 6 Managing File Transfers

```swift
// WCSessionFileTransfer接收完成，接收失败会有报错信息
@available(iOS 9.0, *)
optional public func session(_ session: WCSession, didFinish fileTransfer: WCSessionFileTransfer, error: Error?)

// WCSessionFile接收完成
@available(iOS 9.0, *)
optional public func session(_ session: WCSession, didReceive file: WCSessionFile)
```

&#160;

----------

# Appendix

## Related Documentation

[WCSessionDelegate](https://developer.apple.com/reference/watchconnectivity/WCSessionDelegate)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-09 | 博文完成 |

## Copyright

CSDN：[http://blog.csdn.net/y550918116j](http://blog.csdn.net/y550918116j)

GitHub：[https://github.com/937447974](https://github.com/937447974)