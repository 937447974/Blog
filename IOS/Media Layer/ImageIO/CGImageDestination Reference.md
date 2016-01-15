CGImageDestination用于将照片写入到用户的磁盘中，在写入前可修改元数据。

#1 Creating Image Destinations

```swift
/// 通过Quartz生成CGImageDestination
@available(iOS 4.0, *)
public func CGImageDestinationCreateWithDataConsumer(consumer: CGDataConsumer, _ type: CFString, _ count: Int, _ options: CFDictionary?) -> CGImageDestination?

/// 通过元数据生成CGImageDestination
@available(iOS 4.0, *)
public func CGImageDestinationCreateWithData(data: CFMutableData, _ type: CFString, _ count: Int, _ options: CFDictionary?) -> CGImageDestination?

/// 通过路径生成CGImageDestination
@available(iOS 4.0, *)
public func CGImageDestinationCreateWithURL(url: CFURL, _ type: CFString, _ count: Int, _ options: CFDictionary?) -> CGImageDestination?
```

#2 Adding Images

```swift
/// 增加照片
@available(iOS 4.0, *)
public func CGImageDestinationAddImage(idst: CGImageDestination, _ image: CGImage, _ properties: CFDictionary?)

/// 增加照片到指定的位置，并设置相关属性
@available(iOS 4.0, *)
public func CGImageDestinationAddImageFromSource(idst: CGImageDestination, _ isrc: CGImageSource, _ index: Int, _ properties: CFDictionary?)

/// 增加照片
@available(iOS 7.0, *)
public func CGImageDestinationAddImageAndMetadata(idst: CGImageDestination, _ image: CGImage, _ metadata: CGImageMetadata?, _ options: CFDictionary?)

/// 替换要写入的数据源
@available(iOS 7.0, *)
public func CGImageDestinationCopyImageSource(idst: CGImageDestination, _ isrc: CGImageSource, _ options: CFDictionary?, _ err: UnsafeMutablePointer<Unmanaged<CFError>?>) -> Bool
```

#3 Getting Type Identifiers

```swift
/// 获取唯一标示符
@available(iOS 4.0, *)
public func CGImageDestinationGetTypeID() -> CFTypeID

/// 获取类型数组
@available(iOS 4.0, *)
public func CGImageDestinationCopyTypeIdentifiers() -> CFArray
```

#4 Setting Properties

```swift
/// 设置属性
@available(iOS 4.0, *)
public func CGImageDestinationSetProperties(idst: CGImageDestination, _ properties: CFDictionary?)
```

#5 Finalizing an Image Destination

```swift
/// 写入照片
@available(iOS 4.0, *)
public func CGImageDestinationFinalize(idst: CGImageDestination) -> Bool
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[CGImageDestination Reference](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CGImageDestination/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-15 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog