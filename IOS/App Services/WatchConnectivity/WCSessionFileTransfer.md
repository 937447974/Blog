WCSessionFileTransfer主要用于管理WCSession传输文件。

#Symbols

##1 Getting the File Information

```swift
// 船速得文件信息
open var file: WCSessionFile { get }
```

##2 Managing the File Transfer

```swift
// 是否正在发送数据
open var isTransferring: Bool { get }
// 取消
open func cancel()
```

&#160;

----------

#Appendix

##Related Documentation

[WCSessionFileTransfer](https://developer.apple.com/reference/watchconnectivity/wcsessionfiletransfer)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-09 | 博文完成 |

##Copyright

CSDN：[http://blog.csdn.net/y550918116j](http://blog.csdn.net/y550918116j)

GitHub：[https://github.com/937447974](https://github.com/937447974)