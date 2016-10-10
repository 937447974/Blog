1. [Getting Specific Operation Queues](#1)
2. [Managing Operations in the Queue](#2)
3. [Managing the Execution of Operations](#3)
4. [Suspending Operations](#4)
5. [Configuring the Queue](#5)

----

NSOperationQueue是NSOperation的操作队列，主要用于管理NSOperation子类的执行。NSOperationQueue是按照顺序执行相关NSOperation操作。

#<a id="1">1 Getting Specific Operation Queues

```swift
/// 获取当前队列
@available(iOS 4.0, *)
public class func currentQueue() -> NSOperationQueue?

/// 获取主队列
@available(iOS 4.0, *)
public class func mainQueue() -> NSOperationQueue
```

#<a id="2">2 Managing Operations in the Queue

```swift
/// 添加一个NSOperation
public func addOperation(op: NSOperation)
    
/// 添加NSOperation组到队列中，并记录是否等待。
///
/// - parameter ops : [NSOperation]
/// - parameter wait : 是否等待其他操作执行完毕
///
/// - returns: void
@available(iOS 4.0, *)
public func addOperations(ops: [NSOperation], waitUntilFinished wait: Bool)
    
/// 添加block操作到队列中
@available(iOS 4.0, *)
public func addOperationWithBlock(block: () -> Void)
    
/// 获取所有操作
public var operations: [NSOperation] { get }
    
/// 队列中的队列数
@available(iOS 4.0, *)
public var operationCount: Int { get }

/// 取消所有队列
public func cancelAllOperations()
    
/// 阻塞当前线程,直到所有的操作执行完毕
public func waitUntilAllOperationsAreFinished()    
```

#<a id="3">3 Managing the Execution of Operations

```swift
/// 队列的级别
@available(iOS 8.0, *)
public var qualityOfService: NSQualityOfService
    
/// 同时执行的最大操作数
public var maxConcurrentOperationCount: Int
```

#<a id="4">4 Suspending Operations

```swift
/// 是否暂停队列
public var suspended: Bool
```

#<a id="5">5 Configuring the Queue

```swift
/// 队列名
@available(iOS 4.0, *)
public var name: String?

/// 操作队列的线程
@available(iOS 8.0, *)
unowned(unsafe) public var underlyingQueue: dispatch_queue_t?
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[NSOperationQueue Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/NSOperationQueue_class/index.html)

[Concurrency Programming Guide](https://developer.apple.com/library/ios/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html)

[Simple and Reliable Threading with NSOperation](https://developer.apple.com/library/ios/technotes/tn2109/_index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-09 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog