[Photos(PHAssetChangeRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHAssetChangeRequest).md)

[Photos(PHAssetCreationRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHAssetCreationRequest).md)

[Photos(PHAssetCollectionChangeRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHAssetCollectionChangeRequest).md)

[Photos(PHCollectionListChangeRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHCollectionListChangeRequest).md)

----

PHCollectionListChangeRequest可创建、删除和修改PHCollectionList对象。

#1 Adding New Collection Lists

```swift
/// 创建PHCollectionList
///
/// - parameter title : PHCollectionList名
///
/// - returns: PHCollectionListChangeRequest
public class func creationRequestForAssetCollectionWithTitle(title: String) -> Self
    
/// 新创建的PHCollectionList
public var placeholderForCreatedAssetCollection: PHObjectPlaceholder { get }
```

#2 Deleting Collection Lists

```swift
/// 删除PHCollectionList
///
/// - parameter collectionLists : [PHCollectionList]
///
/// - returns: void
public class func deleteCollectionLists(collectionLists: NSFastEnumeration)
```

#3 Modifying Collection Lists

```swift
/// 通过PHCollectionList初始化PHCollectionListChangeRequest
 public convenience init?(forCollectionList collectionList: PHCollectionList)
    
/// 初始化使用PHFetchResult替换PHCollectionList内部数据
public convenience init?(forCollectionList collectionList: PHCollectionList, childCollections: PHFetchResult)
    
/// 集合名
public var title: String
    
// A PHCollection can only belong to a single parent PHCollection
/// 增加[PHCollection]
public func addChildCollections(collections: NSFastEnumeration)
/// 插入[PHCollection]
public func insertChildCollections(collections: NSFastEnumeration, atIndexes indexes: NSIndexSet)
/// 删除[PHCollection]
public func removeChildCollections(collections: NSFastEnumeration)
/// 删除指定位置的[PHCollection]
public func removeChildCollectionsAtIndexes(indexes: NSIndexSet)
/// 替换指定位置的[PHCollection]
public func replaceChildCollectionsAtIndexes(indexes: NSIndexSet, withChildCollections collections: NSFastEnumeration)
    
/// 移动PHCollectionList内部数据
public func moveChildCollectionsAtIndexes(indexes: NSIndexSet, toIndex: Int)
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHAssetCreationRequest Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHAssetCreationRequest_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-07 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog