NSScanner（OC）、Scanner（Swift）是一个扫描工具，主要用于扫描NSString。

##1 Creating a Scanner

```swift
// 初始化
public init(string: String)
open class func localizedScanner(with string: String) -> Any
```

##2 Getting a Scanner’s String

```swift
// 当前扫描的String
open var string: String { get }
```

##3 Configuring a Scanner

```swift
// 开始扫描的位置
open var scanLocation: Int
// 忽略的扫描数据
open var charactersToBeSkipped: CharacterSet?
// 扫描的字符是否区分
open var caseSensitive: Bool
// 扫描的地方
open var locale: Any?
```

##4 Scanning a String

```swift
// 扫描十进制
open func scanInt32(_ result: UnsafeMutablePointer<Int32>?) -> Bool
@available(iOS 2.0, *)
open func scanInt(_ result: UnsafeMutablePointer<Int>?) -> Bool
open func scanInt64(_ result: UnsafeMutablePointer<Int64>?) -> Bool
@available(iOS 7.0, *)
open func scanUnsignedLongLong(_ result: UnsafeMutablePointer<UInt64>?) -> Bool
open func scanFloat(_ result: UnsafeMutablePointer<Float>?) -> Bool
open func scanDouble(_ result: UnsafeMutablePointer<Double>?) -> Bool

// 扫描十六进制
open func scanHexInt32(_ result: UnsafeMutablePointer<UInt32>?) -> Bool // Optionally prefixed with "0x" or "0X"
@available(iOS 2.0, *)
open func scanHexInt64(_ result: UnsafeMutablePointer<UInt64>?) -> Bool // Optionally prefixed with "0x" or "0X"
@available(iOS 2.0, *)
open func scanHexFloat(_ result: UnsafeMutablePointer<Float>?) -> Bool // Corresponding to %a or %A formatting. Requires "0x" or "0X" prefix.
@available(iOS 2.0, *)
open func scanHexDouble(_ result: UnsafeMutablePointer<Double>?) -> Bool // Corresponding to %a or %A formatting. Requires "0x" or "0X" prefix.

// 扫描String
open func scanString(_ string: String, into result: AutoreleasingUnsafeMutablePointer<NSString?>?) -> Bool
open func scanCharacters(from set: CharacterSet, into result: AutoreleasingUnsafeMutablePointer<NSString?>?) -> Bool

// 扫描以String结束，true会获取scanLocation到扫描处的数据,false会获取scanLocation之后的所有数据
open func scanUpTo(_ string: String, into result: AutoreleasingUnsafeMutablePointer<NSString?>?) -> Bool
open func scanUpToCharacters(from set: CharacterSet, into result: AutoreleasingUnsafeMutablePointer<NSString?>?) -> Bool

// 是否已扫描所有数据
open var isAtEnd: Bool { get }
```

&#160;

----------

#Appendix

##Related Documentation

[NSScanner](https://developer.apple.com/reference/foundation/nsscanner?language=objc)


##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-09-29 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974