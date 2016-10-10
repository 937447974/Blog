[Photos(PHLivePhoto)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHLivePhoto).md)

[PhotosUI(PHLivePhotoView)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/PhotosUI/PhotosUI(PHLivePhotoView).md)

---

PHLivePhotoView用于显示用户的生活照片。

#1 Choosing a Live Photo to Display

```swift
public var livePhoto: PHLivePhoto?
```

#2 Managing Playback

```swift
/// 是否有声音
public var muted: Bool
/// 控制回放的手势
public var playbackGestureRecognizer: UIGestureRecognizer { get }
```

#3 Responding to Playback Events

```swift
/// 开始播放生活照片
public func startPlaybackWithStyle(playbackStyle: PHLivePhotoViewPlaybackStyle)
/// 停止播放
public func stopPlayback()
```

#4 Manually Playing Live Photo Content

```swift
/// 代理，监听播放开始和结束
weak public var delegate: PHLivePhotoViewDelegate?
```

#5 Accessing User Interface Icons for Live Photos

```swift
/// 获取显示照片
///
/// - parameter badgeOptions : PHLivePhotoBadgeOptions
///
/// - returns: UIImage
public class func livePhotoBadgeImageWithOptions(badgeOptions: PHLivePhotoBadgeOptions) -> UIImage
```

#6 实战演示

下面演示了显示并播放生活照片的源代码

```swift
if self.asset.mediaSubtypes == PHAssetMediaSubtype.PhotoLive {
    let options = PHLivePhotoRequestOptions()
    options.deliveryMode = PHImageRequestOptionsDeliveryMode.HighQualityFormat
    options.networkAccessAllowed = true
    options.progressHandler = { (progress: Double, error: NSError?, stop: UnsafeMutablePointer<ObjCBool>, info: [NSObject : AnyObject]?) -> Void in
        print(progress)
    }
    PHImageManager.defaultManager().requestLivePhotoForAsset(self.asset, targetSize: self.targetSize(), contentMode: PHImageContentMode.AspectFit, options: options, resultHandler: { (livePhoto: PHLivePhoto?, info: [NSObject : AnyObject]?) -> Void in
        self.livePhotoView.livePhoto = livePhoto
        if let degradedKeyinfo = info?[PHImageResultIsDegradedKey] {
            if !degradedKeyinfo.boolValue && !self.playing {
                self.livePhotoView.startPlaybackWithStyle(PHLivePhotoViewPlaybackStyle.Hint)
            }
        }
    })
}
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHLivePhoto Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHLivePhoto_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-07 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog