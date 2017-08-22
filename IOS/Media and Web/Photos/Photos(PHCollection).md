PHCollection可以认为是一个抽象类，代表共享资产的集合。多数情况下我们是在操作它的子类。

- PHAssetCollection：照片或视频集合（照片App中的相薄）。
- PHCollectionList：资产的分组（照片App中的照片）。

# 1 获取集合

```swift
/// 从PHCollectionList中获取PHCollection
///
/// - parameter collectionList : PHCollectionList
/// - parameter options : PHFetchOptions?
///
/// - returns: PHFetchResult<PHCollection的子类>
public class func fetchCollectionsInCollectionList(collectionList: PHCollectionList, options: PHFetchOptions?) -> PHFetchResult
    
/// 获取用户创建的集合
///
/// - parameter options : PHFetchOptions?
///
/// - returns: PHFetchResult<PHCollection的子类>
public class func fetchTopLevelUserCollectionsWithOptions(options: PHFetchOptions?) -> PHFetchResult
```

# 2 读取集合元数据

```swift
/// 标题
var localizedTitle: String? { get }
```

# 3 确定集合的功能

```swift
/// 能否包含资产
public var canContainAssets: Bool { get }
/// 能否包含集合
public var canContainCollections: Bool { get }

/// 能否编辑
///
/// - parameter anOperation : PHCollectionEditOperation
///
/// - returns: Bool
public func canPerformEditOperation(anOperation: PHCollectionEditOperation) -> Bool
```

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHObject Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHObject_Class/index.html)

[PHCollection Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHCollection_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-05 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog