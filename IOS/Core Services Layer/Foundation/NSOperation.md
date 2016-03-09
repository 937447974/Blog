1. [Executing the Operation](#1)
2. [Canceling Operations](#2)
3. [Getting the Operation Status](#3)
4. [Managing Dependencies](#4)
5. [Configuring the Execution Priority](#5)
6. [Waiting on an Operation Object](#6)

-----

NSOperation是一个任务的抽象接口，我们主要使用它的子类NSInvocationOperation和NSBlockOperation。我们也可以自定义Operation，继承NSOperation即可。

**Subclassing Notes**

对于继承NSOperation，非并发的Operation，只需实现：

- main

对于并发的Operation，需要实现如下几种方法：

- start
- asynchronous
- executing
- finished


#<a id="1">1 Executing the Operation

```swift
/// 开始执行方法
public func start()

/// 执行的非并发操作
public func main()
    
/// 操作执行完毕的回调
@available(iOS 4.0, *)
public var completionBlock: (() -> Void)?
```

#<a id="2">2 Canceling Operations

```swift
/// 取消任务
public func cancel()
```

#<a id="3">3 Getting the Operation Status

```swift
/// 是否取消
public var cancelled: Bool { get }
/// 是否执行
public var executing: Bool { get }
/// 是否执行完成
public var finished: Bool { get }
/// 是否并发的
public var concurrent: Bool { get }
/// 是否异步操作执行其任务
@available(iOS 7.0, *)
public var asynchronous: Bool { get }
/// 任务能否执行
public var ready: Bool { get }
/// 任务名
@available(iOS 8.0, *)
public var name: String?
```

#<a id="4">4 Managing Dependencies

```swift
/// 添加子任务
public func addDependency(op: NSOperation)
/// 去掉子任务
public func removeDependency(op: NSOperation)
/// 子任务
public var dependencies: [NSOperation] { get }
```

#<a id="5">5 Configuring the Execution Priority

```swift
/// 任务优先级
public var queuePriority: NSOperationQueuePriority

/// 线程优先级
@available(iOS, introduced=4.0, deprecated=8.0)
public var threadPriority: Double

/// 资源级别
@available(iOS 8.0, *)
public var qualityOfService: NSQualityOfService
```

#<a id="6">6 Waiting on an Operation Object

```swift
/// 阻塞当前线程的执行，直到完成其任务
@available(iOS 4.0, *)
public func waitUntilFinished()
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[NSOperation Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/NSOperation_class/index.html)

[Concurrency Programming Guide](https://developer.apple.com/library/ios/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-09 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/927337973/Blog