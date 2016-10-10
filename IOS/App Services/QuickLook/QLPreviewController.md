**1 [QLPreviewController](#1)**

1. [Configuring a Quick Look Preview Controller](#1.1)
2. [Managing Item Previews](#1.2)

**2 [QLPreviewControllerDataSource](#2)**

1. [Providing Data to a Quick Look Preview Controller](#2.1)

**3 [QLPreviewControllerDelegate](#3)**

1. [Responding to Preview Requests](#3.1)
2. [Responding to User Actions](#3.2)

----

#<a id="1">1 QLPreviewController

QLPreviewController是预览文件的UIViewController。其支持的文件类型有：

1. iWork documents
2. Microsoft Office documents (Office ‘97 and newer)
3. Rich Text Format (RTF) documents
4. PDF files
5. Images
6. Text files whose uniform type identifier (UTI) conforms to the public.text type (see Uniform Type Identifiers Reference)
7. Comma-separated value (csv) files

##<a id="1.1">1.1 Configuring a Quick Look Preview Controller

```swift
/// QLPreviewControllerDataSource代理
weak public var dataSource: QLPreviewControllerDataSource?
/// QLPreviewControllerDelegate代理
weak public var delegate: QLPreviewControllerDelegate?
```

##<a id="1.2">1.2 Managing Item Previews

```swift
/// 文件能否显示
public class func canPreviewItem(item: QLPreviewItem) -> Bool
    
/// 刷新数据
public func reloadData()
    
/// 更新当前预览的类
public func refreshCurrentPreviewItem()
    
/// 当前显示文件所属位置
public var currentPreviewItemIndex: Int

/// 显示的所有文件
public var currentPreviewItem: QLPreviewItem? { get }
```

#<a id="2">2 QLPreviewControllerDataSource

QLPreviewControllerDataSource用于显示文件。

##<a id="2.1">2.1 Providing Data to a Quick Look Preview Controller

```swift
/// 可显示的Item数量
@available(iOS 4.0, *)
public func numberOfPreviewItemsInPreviewController(controller: QLPreviewController) -> Int
    
/// 获取要显示的文件QLPreviewItem
@available(iOS 4.0, *)
public func previewController(controller: QLPreviewController, previewItemAtIndex index: Int) -> QLPreviewItem
```

#<a id="3">3 QLPreviewControllerDelegate

QLPreviewControllerDelegate用于操作文件。

##<a id="3.1">3.1 Responding to Preview Requests

```swift
/// QLPreviewController将要关闭
@available(iOS 4.0, *)
optional public func previewControllerWillDismiss(controller: QLPreviewController)
    
/// QLPreviewController关闭
@available(iOS 4.0, *)
optional public func previewControllerDidDismiss(controller: QLPreviewController)
    
/// 放大效果
@available(iOS 4.0, *)
optional public func previewController(controller: QLPreviewController, frameForPreviewItem item: QLPreviewItem, inSourceView view: AutoreleasingUnsafeMutablePointer<UIView?>) -> CGRect
    
/// 过渡动画展示的图片
@available(iOS 4.0, *)
optional public func previewController(controller: QLPreviewController, transitionImageForPreviewItem item: QLPreviewItem, contentRect: UnsafeMutablePointer<CGRect>) -> UIImage
```

##<a id="3.2">3.2 Responding to User Actions

```swift
/// 能否打开URL
@available(iOS 4.0, *)
optional public func previewController(controller: QLPreviewController, shouldOpenURL url: NSURL, forPreviewItem item: QLPreviewItem) -> Bool
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Quick Look Framework Reference for iOS](https://developer.apple.com/library/ios/documentation/QuickLook/Reference/QuickLookFrameworkReference_iPhoneOS/index.html)

[QLPreviewController Class Reference](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/QLPreviewController_Class/index.html)

[QLPreviewControllerDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/QLPreviewControllerDelegate_Protocol/index.html)

[QLPreviewControllerDataSource Protocol Reference](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/QLPreviewControllerDataSource_Protocol/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-24 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog