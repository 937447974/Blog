[Photos(PHChange)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHChange).md)

[Photos(PHObjectChangeDetails)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHObjectChangeDetails).md)

[Photos(PHFetchResultChangeDetails)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHFetchResultChangeDetails).md)

---

当我们通过PHPhotoLibrary的`registerChangeObserver(_ observer: PHPhotoLibraryChangeObserver)`方法监听用户的照片库时，在其回调协议中`photoLibraryDidChange(_ changeInstance: PHChange)`通知当前APP。简而言之就是照片库提供PHChange对象来通知应用程序照片库的变动。

#1 获取变化

```swift
/// 判断是否有变化
///
/// - parameter object : PHAsset, PHAssetCollection, or PHCollectionList object.
///
/// - returns: PHObjectChangeDetails
public func changeDetailsForObject(object: PHObject) -> PHObjectChangeDetails?
    
/// 判断集合是否有变化
///
/// - parameter object : PHFetchResult
///
/// - returns: PHFetchResultChangeDetails
public func changeDetailsForFetchResult(object: PHFetchResult) -> PHFetchResultChangeDetails?
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHChange Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHChange_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-05 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog