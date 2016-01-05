PHImageManager是PHAsset管理器，它能从PHAsset获取你想要的数据，如UIImage、NSData等。

#1 获取PHImageManager

```swift
/// 获取单例PHImageManager
///
/// - returns: PHImageManager
public class func defaultManager() -> PHImageManager
```

#2 获取照片

```swift
/// 获取UIImage
///
/// - parameter asset : PHAsset
/// - parameter targetSize : CGSize图片尺寸
/// - parameter contentMode : PHImageContentMode图片模式
/// - parameter options : PHImageRequestOptions加载方式
/// - parameter resultHandler : (UIImage?, [NSObject : AnyObject]?) -> Void 闭包回调返回数据
///
/// - returns: PHImageRequestID加载标示符
public func requestImageForAsset(asset: PHAsset, targetSize: CGSize, contentMode: PHImageContentMode, options: PHImageRequestOptions?, resultHandler: (UIImage?, [NSObject : AnyObject]?) -> Void) -> PHImageRequestID
    
/// 获取NSData
///
/// - parameter asset : PHAsset
/// - parameter contentMode : PHImageContentMode加载方式
/// - parameter resultHandler : (NSData?, String?, UIImageOrientation, [NSObject : AnyObject]?) -> Void 闭包回调返回数据
///
/// - returns: PHImageRequestID
public func requestImageDataForAsset(asset: PHAsset, options: PHImageRequestOptions?, resultHandler: (NSData?, String?, UIImageOrientation, [NSObject : AnyObject]?) -> Void) -> PHImageRequestID
```

#3 获取视频

```swift
/// 获取视频AVPlayerItem
///
/// - parameter asset : PHAsset
/// - parameter options : PHVideoRequestOptions加载方式
/// - parameter resultHandler : (AVPlayerItem?, [NSObject : AnyObject]?) -> Void 闭包返回数据
///
/// - returns: PHImageRequestID加载标示符
public func requestPlayerItemForVideo(asset: PHAsset, options: PHVideoRequestOptions?, resultHandler: (AVPlayerItem?, [NSObject : AnyObject]?) -> Void) -> PHImageRequestID
    
/// 获取视频AVPlayerItem
///
/// - parameter asset : PHAsset
/// - parameter options : PHVideoRequestOptions加载方式
/// - parameter exportPreset : 名称
/// - parameter resultHandler : (AVAssetExportSession?, [NSObject : AnyObject]?) -> Void 闭包返回数据
///
/// - returns: PHImageRequestID加载标示符
public func requestExportSessionForVideo(asset: PHAsset, options: PHVideoRequestOptions?, exportPreset: String, resultHandler: (AVAssetExportSession?, [NSObject : AnyObject]?) -> Void) -> PHImageRequestID
    
/// 获取AVAsset
///
/// - parameter asset : PHAsset
/// - parameter options : PHVideoRequestOptions加载方式
/// - parameter resultHandler : (AVAsset?, AVAudioMix?, [NSObject : AnyObject]?) -> Void 闭包返回数据
///
/// - returns: PHImageRequestID加载标示符
public func requestAVAssetForVideo(asset: PHAsset, options: PHVideoRequestOptions?, resultHandler: (AVAsset?, AVAudioMix?, [NSObject : AnyObject]?) -> Void) -> PHImageRequestID
```

#4 获取生活照片

```swift
/// 获取生活照片
///
/// - parameter asset : PHAsset
/// - parameter targetSize : CGSize 目标大小
/// - parameter contentMode : PHImageContentMode 显示模式
/// - parameter options : PHLivePhotoRequestOptions 加载设置
/// - parameter resultHandler: (PHLivePhoto?, [NSObject : AnyObject]?) -> Void 闭包返回数据
///
/// - returns: PHImageRequestID
@available(iOS 9.1, *)
public func requestLivePhotoForAsset(asset: PHAsset, targetSize: CGSize, contentMode: PHImageContentMode, options: PHLivePhotoRequestOptions?, resultHandler: (PHLivePhoto?, [NSObject : AnyObject]?) -> Void) -> PHImageRequestID
```

#5 取消加载

```swift
/// 取消获取数据
///
/// - parameter requestID : PHImageRequestID 加载标示符
///
/// - returns: void
public func cancelImageRequest(requestID: PHImageRequestID)
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHImageManager Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHImageManager_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-05 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog