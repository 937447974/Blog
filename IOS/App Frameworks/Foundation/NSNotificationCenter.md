NSNotificationCenter为通知管理中心，通过它我们可以注册通知监听、取消通知监听以及发出通知。

# 1 Getting the Notification Center

```swift
/// 默认通知中心
public class func defaultCenter() -> NSNotificationCenter
```

# 2 Managing Notification Observers

```swift
/// block接受通知
///
/// - parameter name: 通知名
/// - parameter obj: 关联对象
/// - parameter queue: 队列
/// - parameter block: 通知回调
///
/// - returns: NSObjectProtocol
@available(iOS 4.0, *)
public func addObserverForName(name: String?, object obj: AnyObject?, queue: NSOperationQueue?, usingBlock block: (NSNotification) -> Void) -> NSObjectProtocol
    
/// Selector接收通知
///
/// - parameter observer: 目标类self
/// - parameter aSelector: Selector
/// - parameter aName: String
/// - parameter object: 关联对象
///
/// - returns: void
public func addObserver(observer: AnyObject, selector aSelector: Selector, name aName: String?, object anObject: AnyObject?)

/// 注销通知
///
/// - parameter observer : 目标类self
///
/// - returns: void
public func removeObserver(observer: AnyObject)
    
/// 注销通知
///
/// - parameter observer : 目标类self
/// - parameter aName : 通知名
/// - parameter anObject : 关联对象
///
/// - returns: void
public func removeObserver(observer: AnyObject, name aName: String?, object anObject: AnyObject?)
```

# 3 Posting Notifications

```swift
/// 发出通知
///
/// - parameter notification : NSNotification
///
/// - returns: void
public func postNotification(notification: NSNotification)
    
/// 发出通知
///
/// - parameter aName : 通知名
/// - parameter anObject : 关联的对象
///
/// - returns: void
public func postNotificationName(aName: String, object anObject: AnyObject?)
    
/// 发出通知
///
/// - parameter aName : 通知名
/// - parameter anObject : 关联的对象
/// - parameter userInfo : 携带的数据
///
/// - returns: void
public func postNotificationName(aName: String, object anObject: AnyObject?, userInfo aUserInfo: [NSObject : AnyObject]?)
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[NSNotificationCenter Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSNotificationCenter_Class/index.html)

[Notification Programming Topics](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Notifications/Introduction/introNotifications.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-15 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog