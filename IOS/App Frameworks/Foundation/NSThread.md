NSThread对象控制线程的执行。当我们需要执行一个冗长的任务，又不希望它阻塞应用程序的执行时，是用NSThread是特别有用的。

NSThread的优缺点具有以下两点。

1. 优点：NSThread比其他Cocoa NSOperation和GCD轻量级。
2. 缺点：需要自己管理线程的生命周期，线程同步。线程同步对数据的加锁会有一定的系统开销。

# 1 Initializing an NSThread Object

```swift
// 初始化NSThread
@available(iOS 2.0, *)
public init()

// 初始化NSThread
@available(iOS 2.0, *)
public convenience init(target: AnyObject, selector: Selector, object argument: AnyObject?)
```

# 2 Starting a Thread

```swift
// 分离一个线程并执行相应的代码
public class func detachNewThreadSelector(selector: Selector, toTarget target: AnyObject, withObject argument: AnyObject?)

// 开始执行
@available(iOS 2.0, *)
public func start()
    
// 线程的主体
@available(iOS 2.0, *)
public func main() // thread body method
```

# 3 Stopping a Thread

```swift
// 阻塞当前线程,直到指定的时间。
public class func sleepUntilDate(date: NSDate)

// 阻塞当前线程一定的时间
public class func sleepForTimeInterval(ti: NSTimeInterval)

// 终止线程
public class func exit()

// 线程取消，通知线程终止
@available(iOS 2.0, *)
public func cancel()
```

# 4 Determining the Thread’s Execution State

```swift
// 是否正在执行
@available(iOS 2.0, *)
public var executing: Bool { get }

// 是否执行完毕
@available(iOS 2.0, *)
public var finished: Bool { get }

// 是否取消
@available(iOS 2.0, *)
public var cancelled: Bool { get }
```

# 5 Working with the Main Thread

```swift
// 线程是否为主线程
@available(iOS 2.0, *)
public var isMainThread: Bool { get }
    
// 当前线程是否为主线程
@available(iOS 2.0, *)
public class func isMainThread() -> Bool
    
// 获取主线程
@available(iOS 2.0, *)
public class func mainThread() -> NSThread
```

# 6 Querying the Environment

```swift
// 获取当前线程
public class func currentThread() -> NSThread
    
// 当前应用能否执行多线程程序
public class func isMultiThreaded() -> Bool
    
// 获取堆栈空间中的线程地址
@available(iOS 2.0, *)
public class func callStackReturnAddresses() -> [NSNumber]
    
// 返回调用的堆栈符号
@available(iOS 4.0, *)
public class func callStackSymbols() -> [String]
```

# 7 Working with Thread Properties

```swift
// 线程字典
public var threadDictionary: NSMutableDictionary { get }

// 接收器的名
@available(iOS 2.0, *)
public var name: String?
    
// 占用的堆空间、bytes
@available(iOS 2.0, *)
public var stackSize: Int
```

# 8 Working with Thread Priorities

```swift
// 当前线程优先级0~1.0
public class func threadPriority() -> Double

// 设置线程优先级0~1.0
public class func setThreadPriority(p: Double) -> Bool

// 线程优先级0~1.0
@available(iOS 4.0, *)
public var threadPriority: Double // To be deprecated; use qualityOfService below

// 线程优先级
@available(iOS 8.0, *)
public var qualityOfService: NSQualityOfService // read-only after the thread is started  
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[NSThread Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSThread_Class/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-30 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog