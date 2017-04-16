NSNotificationCenter发出通知的过程中会发出一个一个NSNotification对象。

# 1 Creating Notifications

```swift
/// 创建NSNotification
///
/// - parameter aName : 通知名
/// - parameter anObject : 通知关联的对象
///
/// - returns: NSNotification
public convenience init(name aName: String, object anObject: AnyObject?)
    
/// 创建NSNotification
///
/// - parameter name : 通知名
/// - parameter object : 通知关联的对象
/// - parameter userInfo : 通知携带的数据
///
/// - returns: NSNotification
@available(iOS 4.0, *)
public init(name: String, object: AnyObject?, userInfo: [NSObject : AnyObject]?)
```

# 2 Getting Notification Information

```swift
/// 通知名
public var name: String { get }
/// 通知关联的对象
public var object: AnyObject? { get }
/// 通知携带的数据
public var userInfo: [NSObject : AnyObject]? { get }
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[NSNotification Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSNotification_Class/index.html)

[Notification Programming Topics](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Notifications/Introduction/introNotifications.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-15 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog