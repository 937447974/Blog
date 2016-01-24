QLPreviewItem是一个协议，当我们想在QLPreviewController显示一个文件时，需要自定义实体类，实现QLPreviewItem协议。

#1 Providing an Item URL

```swift
/// 文件路径
public var previewItemURL: NSURL { get }
```

#2 Providing an Item Title

```swift    
/// 文件标题
optional public var previewItemTitle: String? { get }
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Quick Look Framework Reference for iOS](https://developer.apple.com/library/ios/documentation/QuickLook/Reference/QuickLookFrameworkReference_iPhoneOS/index.html)

[QLPreviewItem Protocol Reference for iOS](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/QLPreviewItem_Protocol_iPhoneOS/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-24 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog