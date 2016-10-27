NSSortDescriptor即排序描述。

#Symbols

##1 Initializing a Sort Descriptor

```swift
// 初始化
public init(key: String?, ascending: Bool)
public init(key: String?, ascending: Bool, selector: Selector?)
```

##2 Getting Information About a Sort Descriptor

```swift
// 关键字
open var key: String? { get }
// 是否升序
open var ascending: Bool { get }
// 方法监听
open var selector: Selector? { get }
```

##3 Using Sort Descriptors

```swift
// 比较大小
open func compare(_ object1: Any, to object2: Any) -> ComparisonResult
// 排序顺序相反的描述
open var reversedSortDescriptor: Any { get }
@available(iOS 7.0, *)
open func allowEvaluation() // Force a sort descriptor which was securely decoded to allow evaluation
```

##4 Create an NSComparator for the Sort Descriptor

```swift
// 获取比较器
@available(iOS 4.0, *)
open var comparator: Foundation.Comparator { get }
```

&#160;

----------

#Appendix

##Related Documentation

[NSSortDescriptor](https://developer.apple.com/reference/foundation/nssortdescriptor)

[Key-Value Coding Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-26 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974