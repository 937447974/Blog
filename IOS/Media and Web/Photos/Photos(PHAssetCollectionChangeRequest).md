[Photos(PHAssetChangeRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHAssetChangeRequest).md)

[Photos(PHAssetCreationRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHAssetCreationRequest).md)

[Photos(PHAssetCollectionChangeRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHAssetCollectionChangeRequest).md)

[Photos(PHCollectionListChangeRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHCollectionListChangeRequest).md)

----

PHAssetCollectionChangeRequest可创建、删除和修改PHAssetCollection对象。


# 1 Adding New Asset Collections

```swift
/// 创建相册
///
/// - parameter title : 相册名
///
/// - returns: PHAssetCollectionChangeRequest
public class func creationRequestForAssetCollectionWithTitle(title: String) -> Self
    
/// 新创建的相册
public var placeholderForCreatedAssetCollection: PHObjectPlaceholder { get }
```

# 2 Deleting Asset Collections

```swift
/// 删除指定的PHAssetCollection
///
/// - parameter assetCollections : [PHAssetCollection]
///
/// - returns: void
 public class func deleteAssetCollections(assetCollections: NSFastEnumeration)
```

# 3 Modifying Asset Collections

```swift
/// 通过PHAssetCollection初始化PHAssetCollectionChangeRequest
public convenience init?(forAssetCollection assetCollection: PHAssetCollection)
    
/// 初始化使用PHFetchResult替换PHAssetCollection内部数据
public convenience init?(forAssetCollection assetCollection: PHAssetCollection, assets: PHFetchResult)
    
/// 相册名
public var title: String
    
/// 增加[PHAsset]
public func addAssets(assets: NSFastEnumeration)
/// 插入[PHAsset]
public func insertAssets(assets: NSFastEnumeration, atIndexes indexes: NSIndexSet)
/// 删除[PHAsset]
public func removeAssets(assets: NSFastEnumeration)
/// 删除指定位置的[PHAsset]
public func removeAssetsAtIndexes(indexes: NSIndexSet)
/// 替换指定位置的[PHAsset]
public func replaceAssetsAtIndexes(indexes: NSIndexSet, withAssets assets: NSFastEnumeration)
    
/// 移动[PHAsset]到指定的位置
public func moveAssetsAtIndexes(fromIndexes: NSIndexSet, toIndex: Int)
```

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHAssetCreationRequest Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHAssetCreationRequest_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-07 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog