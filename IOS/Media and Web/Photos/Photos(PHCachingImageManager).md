PHCachingImageManager是PHImageManager的子类，主要用于缓存PHAsset，这样可以快速获取照片或视频。

# 1 准备图片

```swift
/// 开始缓存请求
///
/// - parameter assets : [PHAsset]
/// - parameter targetSize : CGSize目标尺寸
/// - parameter contentMode: PHImageContentMode显示模式
/// - parameter options: PHImageRequestOptions加载相关设置
///
/// - returns: void
public func startCachingImagesForAssets(assets: [PHAsset], targetSize: CGSize, contentMode: PHImageContentMode, options: PHImageRequestOptions?)
    
/// 停止缓存请求
///
/// - parameter assets : [PHAsset]
/// - parameter targetSize : CGSize目标尺寸
/// - parameter contentMode: PHImageContentMode显示模式
/// - parameter options: PHImageRequestOptions加载相关设置
///
/// - returns: void
public func stopCachingImagesForAssets(assets: [PHAsset], targetSize: CGSize, contentMode: PHImageContentMode, options: PHImageRequestOptions?)
    
/// 停止所有缓存请求
///
/// - returns: void
public func stopCachingImagesForAllAssets()
```

# 2 设置缓存模式

```swift
/// 是否缓存高质量照片，默认true
public var allowsCachingHighQualityImages: Bool
```

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHImageManager Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHImageManager_Class/index.html)

[PHCachingImageManager Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHCachingImageManager_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-05 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog