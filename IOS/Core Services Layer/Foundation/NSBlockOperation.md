NSBlockOperation是NSOperation的子类，以block的方式添加任务。

#1 Managing the Blocks in the Operation

```swift
/// 初始化NSBlockOperation
public convenience init(block: () -> Void)

/// 添加blcok
public func addExecutionBlock(block: () -> Void)
    
/// 需要执行的block
public var executionBlocks: [() -> Void] { get }
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