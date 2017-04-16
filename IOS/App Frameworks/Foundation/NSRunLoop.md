1. [Accessing Run Loops and Modes](#1)
2. [Managing Timers](#2)
3. [Managing Ports](#3)
4. [Running a Loop](#4)
5. [Scheduling and Canceling Messages](#5)

----

NSRunLoop是一种更加高明的消息处理模式，他就高明在对消息处理过程进行了更好的抽象和封装，这样才能是的你不用处理一些很琐碎很低层次的具体消息的处理，在NSRunLoop中每一个消息就被打包在input source或者是timer source中了。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016030901.jpg)


如图所示，将消息放到循环中，保证每个消息在循环的过程中都能执行。

# <a id="1">1 Accessing Run Loops and Modes

```swift
/// 当前runloop
public class func currentRunLoop() -> NSRunLoop

/// 主runloop
@available(iOS 2.0, *)
public class func mainRunLoop() -> NSRunLoop
    
/// 当前runlooop运行模式
public var currentMode: String? { get }
    
/// 获取底层CFRunLoop
public func getCFRunLoop() -> CFRunLoop

/// 根据执行模式返回下一次执行时间
public func limitDateForMode(mode: String) -> NSDate?
```

# <a id="2">2 Managing Timers

```swift
/// 添加NSTimer到指定的模式
public func addTimer(timer: NSTimer, forMode mode: String)
```

# <a id="3">3 Managing Ports

```swift
/// 添加一个NSPort到指定的模式
public func addPort(aPort: NSPort, forMode mode: String)
/// 移除NSPort
public func removePort(aPort: NSPort, forMode mode: String)
```

# <a id="4">4 Running a Loop

```swift
/// 运行
public func run()

/// 在什么时间节点前运行
public func runUntilDate(limitDate: NSDate)

/// 在指定的时间前指定的模式中运行
public func runMode(mode: String, beforeDate limitDate: NSDate) -> Bool

/// 在指定的时间前指定的模式中循环运行一次
public func acceptInputForMode(mode: String, beforeDate limitDate: NSDate)
```

# <a id="5">5 Scheduling and Canceling Messages

```swift
/// 添加消息任务
///
/// - parameter aSelector : Selector
/// - parameter target : 目标类
/// - parameter arg : 携带数据
/// - parameter order : 优先级
/// - parameter modes : 执行模式
///
/// - returns: void
public func performSelector(aSelector: Selector, target: AnyObject, argument arg: AnyObject?, order: Int, modes: [String])

/// 取消消息任务
public func cancelPerformSelector(aSelector: Selector, target: AnyObject, argument arg: AnyObject?)

/// 取消发送给目标的消息
public func cancelPerformSelectorsWithTarget(target: AnyObject)
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[NSRunLoop Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSRunLoop_Class/index.html)

[Threading Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSRunLoop_Class/index.html)

[【iOS程序启动与运转】- RunLoop个人小结](http://www.jianshu.com/p/37ab0397fec7)

[深入理解RunLoop](http://blog.ibireme.com/2015/05/18/runloop/)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-09 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/927337973/Blog