[Photos(PHLivePhoto)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHLivePhoto).md)

[PhotosUI(PHLivePhotoView)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/PhotosUI/PhotosUI(PHLivePhotoView).md)

---

在IOS9以后，当我们玩手机照相时会发现，明明拍的一张照片，但打开时照片会像gif一样的动，这就是PHLivePhoto对象，也就是生活照片。

生活照片的展示会用到PHLivePhotoView UI对象。

# 1 Inspecting a Live Photo

```swift
/// 像素
public var size: CGSize { get }
```

# 2 Loading a Live Photo from Data Files

```swift
/// 获取PHLivePhoto图片
///
/// - parameter fileURLs : [NSURL]图片地址
/// - parameter image : 正在加载时的静态图片
/// - parameter targetSize : 显示大小
/// - parameter resultHandler: 回调返回数据
///
/// - returns: PHLivePhotoRequestID 加载唯一标示符
public class func requestLivePhotoWithResourceFileURLs(fileURLs: [NSURL], placeholderImage image: UIImage?, targetSize: CGSize, contentMode: PHImageContentMode, resultHandler: (PHLivePhoto?, [NSObject : AnyObject]) -> Void) -> PHLivePhotoRequestID
    
/// 取消获取PHLivePhoto
///
/// - parameter requestID : PHLivePhotoRequestID
///
/// - returns: void
public class func cancelLivePhotoRequestWithRequestID(requestID: PHLivePhotoRequestID)
```

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHLivePhoto Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHLivePhoto_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-07 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog