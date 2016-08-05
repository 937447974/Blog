NSTimer是一个计时器，可通过它快速实现相关倒计时需求。

##1 Creating a Timer

```swift
public /*not inherited*/ init(timeInterval ti: NSTimeInterval, invocation: NSInvocation, repeats yesOrNo: Bool)
public class func scheduledTimerWithTimeInterval(ti: NSTimeInterval, invocation: NSInvocation, repeats yesOrNo: Bool) -> NSTimer

public /*not inherited*/ init(timeInterval ti: NSTimeInterval, target aTarget: AnyObject, selector aSelector: Selector, userInfo: AnyObject?, repeats yesOrNo: Bool)
public class func scheduledTimerWithTimeInterval(ti: NSTimeInterval, target aTarget: AnyObject, selector aSelector: Selector, userInfo: AnyObject?, repeats yesOrNo: Bool) -> NSTimer

public init(fireDate date: NSDate, interval ti: NSTimeInterval, target t: AnyObject, selector s: Selector, userInfo ui: AnyObject?, repeats rep: Bool)
```

##2 Firing a Timer

```swift
// 发送消息
public func fire()
```

##3 Stopping a Timer

```swift
// 失效
public func invalidate()
```

##4 Information About a Timer

```swift
// 是否有效
public var valid: Bool { get }
// 携带的信息
public var userInfo: AnyObject? { get }
// 启动时间
@NSCopying public var fireDate: NSDate
// 时间间隔
public var timeInterval: NSTimeInterval { get }
```

##5 Firing Tolerance

```swift
// 因为NSTimer并不完全精准，通过这个值设置误差范围
@available(iOS 7.0, *)
public var tolerance: NSTimeInterval
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[NSTimer Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSTimer_Class/index.html)

[Timer Programming Topics](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Timers/Timers.html)

[Threading Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Multithreading/Introduction/Introduction.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-08-05 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974