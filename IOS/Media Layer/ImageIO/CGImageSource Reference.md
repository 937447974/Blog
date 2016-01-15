CGImageSource用于读取照片，通过它可以获取元数据、属性和缩略图。

#1 Creating an Image Source

```swift
/// 通过缓存数据源获取CGImageSource
@available(iOS 4.0, *)
public func CGImageSourceCreateWithDataProvider(provider: CGDataProvider, _ options: CFDictionary?) -> CGImageSource?

/// 通过CFData获取CGImageSource
@available(iOS 4.0, *)
public func CGImageSourceCreateWithData(data: CFData, _ options: CFDictionary?) -> CGImageSource?

/// 通过路径获取CGImageSource
@available(iOS 4.0, *)
public func CGImageSourceCreateWithURL(url: CFURL, _ options: CFDictionary?) -> CGImageSource?
```

#2 Creating Images From an Image Source

```swift
/// 获取CGImageSource中指定位置的照片CGImage
@available(iOS 4.0, *)
public func CGImageSourceCreateImageAtIndex(isrc: CGImageSource, _ index: Int, _ options: CFDictionary?) -> CGImage?

/// 创建缩略图
@available(iOS 4.0, *)
public func CGImageSourceCreateThumbnailAtIndex(isrc: CGImageSource, _ index: Int, _ options: CFDictionary?) -> CGImage?

/// 创建增强的数据源
@available(iOS 4.0, *)
public func CGImageSourceCreateIncremental(options: CFDictionary?) -> CGImageSource
```

#3 Updating an Image Source

```swift
/// 通过CFData更新数据源
@available(iOS 4.0, *)
public func CGImageSourceUpdateData(isrc: CGImageSource, _ data: CFData, _ final: Bool)

/// 通过CGDataProvider更新数据源
@available(iOS 4.0, *)
public func CGImageSourceUpdateDataProvider(isrc: CGImageSource, _ provider: CGDataProvider, _ final: Bool)
```

#4 Getting Information From an Image Source

```swift

/// 获取唯一标示符
@available(iOS 4.0, *)
public func CGImageSourceGetTypeID() -> CFTypeID

/// 获取类型标示符
@available(iOS 4.0, *)
public func CGImageSourceGetType(isrc: CGImageSource) -> CFString?

/// 获取类型标示符数组
@available(iOS 4.0, *)
public func CGImageSourceCopyTypeIdentifiers() -> CFArray

/// 获取照片数量
@available(iOS 4.0, *)
public func CGImageSourceGetCount(isrc: CGImageSource) -> Int

/// 获取属性
@available(iOS 4.0, *)
public func CGImageSourceCopyProperties(isrc: CGImageSource, _ options: CFDictionary?) -> CFDictionary?

/// 获取指定位置的图片属性
@available(iOS 4.0, *)
public func CGImageSourceCopyPropertiesAtIndex(isrc: CGImageSource, _ index: Int, _ options: CFDictionary?) -> CFDictionary?

/// 获取数据源状态
@available(iOS 4.0, *)
public func CGImageSourceGetStatus(isrc: CGImageSource) -> CGImageSourceStatus

/// 获取指定位置图片的状态
@available(iOS 4.0, *)
public func CGImageSourceGetStatusAtIndex(isrc: CGImageSource, _ index: Int) -> CGImageSourceStatus

/// 获取指定位置的元数据
@available(iOS 7.0, *)
public func CGImageSourceCopyMetadataAtIndex(isrc: CGImageSource, _ index: Int, _ options: CFDictionary?) -> CGImageMetadata?

/// 清楚指定位置的元数据
@available(iOS 7.0, *)
public func CGImageSourceRemoveCacheAtIndex(isrc: CGImageSource, _ index: Int)
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[CGImageSource Reference](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CGImageSource/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-15 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog