NSNotificationQueue是通知队列。NSNotificationCenter发出通知时会加入到通知队列中，最后由NSNotificationQueue发送到目标类目标方法中。

#1 Creating Notification Queues

```swift
/// 创建队列
public init(notificationCenter: NSNotificationCenter)
```

#2 Getting the Default Queue

```swift
/// 默认队列
public class func defaultQueue() -> NSNotificationQueue
```

#3 Managing Notifications

```swift
/// 发出通知
///
/// - parameter notification : 通知
/// - parameter postingStyle : 发布级别
///
/// - returns: void
public func enqueueNotification(notification: NSNotification, postingStyle: NSPostingStyle)
    
/// 发出通知
///
/// - parameter notification : 通知
/// - parameter postingStyle : 发布级别
/// - parameter coalesceMask : 匹配标准
/// - parameter modes : 循环模式
///
/// - returns: void
public func enqueueNotification(notification: NSNotification, postingStyle: NSPostingStyle, coalesceMask: NSNotificationCoalescing, forModes modes: [String]?)
    
/// 发出通知
///
/// - parameter notification : 通知
/// - parameter coalesceMask : 匹配标准
///
/// - returns: void
public func dequeueNotificationsMatching(notification: NSNotification, coalesceMask: Int)
```


&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[NSNotificationQueue Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSNotificationQueue_Class/index.html)

[Notification Programming Topics](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Notifications/Introduction/introNotifications.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-15 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog