[Photos(PHAssetChangeRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHAssetChangeRequest).md)

[Photos(PHAssetCreationRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHAssetCreationRequest).md)

----

PHAssetChangeRequest主要用于创建、删除或修改PHAsset对象。可以理解为它是沟通用户照片库中照片或视频的桥梁。

#1 Adding New Assets

```swift
/// 通过UIImage创建PHAsset
///
/// - parameter image : UIImage
///
/// - returns: PHAssetChangeRequest
public class func creationRequestForAssetFromImage(image: UIImage) -> Self
    
/// 通过照片路径创建PHAsset
///
/// - parameter fileURL : NSURL照片地址
///
/// - returns: PHAssetChangeRequest
public class func creationRequestForAssetFromImageAtFileURL(fileURL: NSURL) -> Self?
    
/// 通过视频路径创建PHAsset
///
/// - parameter fileURL : NSURL视频地址
///
/// - returns: PHAssetChangeRequest
public class func creationRequestForAssetFromVideoAtFileURL(fileURL: NSURL) -> Self?
    
/// 创建的PHAsset对象
public var placeholderForCreatedAsset: PHObjectPlaceholder? { get }
```

#2 Deleting Assets

```swift
/// 删除PHAsset
///
/// - parameter assets: NSFastEnumeration即[PHAsset]
///
/// - returns: void
public class func deleteAssets(assets: NSFastEnumeration)
```

#3 Modifying Assets

```swift
/// 通过PHAsset初始化PHAssetChangeRequest
public convenience init(forAsset asset: PHAsset)

/// 创建时间
public var creationDate: NSDate?
/// 位置
public var location: CLLocation?
/// 是否收藏
public var favorite: Bool
    
/// 是否隐藏
public var hidden: Bool
```

#4 Editing Asset Content

```swift
/// 修改内容
public var contentEditingOutput: PHContentEditingOutput?
/// 回归到初始状态
public func revertAssetContentToOriginal()
```

#5 实战演练

下面演示用户收藏照片或视频的源代码

```swift
func toggleFavoriteForAsset(asset: PHAsset) {
    PHPhotoLibrary.sharedPhotoLibrary().performChanges({
        // Create a change request from the asset to be modified.
        let request = PHAssetChangeRequest(forAsset: asset)
        // Set a property of the request to change the asset itself.
        request.favorite = !asset.favorite
        
        }, completionHandler: { success, error in
            NSLog("Finished updating asset. %@", (success ? "Success." : error))
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

[PHAssetChangeRequest Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHAssetChangeRequest_Class/index.html)

[PHAsset Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHAsset_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-07 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog