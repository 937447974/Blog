WCSessionUserInfoTransfer主要用于管理和记录WCSession传输的UserInfo信息。

#Symbols

##1 Getting the Transfer Information

```swift
// 是否并发信息
open var isCurrentComplicationInfo: Bool { get }
// 携带的信息
open var userInfo: [String : Any] { get }
```

##2 Managing the Transfer Operation

```swift
// 是否正在传输
open var isTransferring: Bool { get }
// 取消传输
open func cancel()
```

&#160;

----------

#Appendix

##Related Documentation

[WCSessionUserInfoTransfer](https://developer.apple.com/reference/watchconnectivity/wcsessionuserinfotransfer)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-09 | 博文完成 |

##Copyright

CSDN：[http://blog.csdn.net/y550918116j](http://blog.csdn.net/y550918116j)

GitHub：[https://github.com/937447974](https://github.com/937447974)