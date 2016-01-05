PHCollectionList是PHCollection的子类，代表照片或视频的组，可以理解为系统“照片”App里面的年度或精选。

#1 获取PHCollectionList

```swift
/// 从指定的PHCollection中获取PHCollectionList
///
/// - parameter collection : PHCollection
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHCollectionList>
public class func fetchCollectionListsContainingCollection(collection: PHCollection, options: PHFetchOptions?) -> PHFetchResult
    
/// 根据唯一标示符获取PHCollectionList
///
/// - parameter identifiers : [String]对应localIdentifier属性
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHCollectionList>
public class func fetchCollectionListsWithLocalIdentifiers(identifiers: [String], options: PHFetchOptions?) -> PHFetchResult
    
/// 根据唯一标示符获取PHCollectionList
///
/// - parameter collectionListType : 集合类型
/// - parameter subtype : 集合详细类型
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHCollectionList>
public class func fetchCollectionListsWithType(collectionListType: PHCollectionListType, subtype: PHCollectionListSubtype, options: PHFetchOptions?) -> PHFetchResult

/// 从PHAssetCollection中指定的PHCollectionListSubtype类型获取PHCollectionList
///
/// - parameter momentListSubtype : PHCollectionListSubtype
/// - parameter containingMoment : PHAssetCollection
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHCollectionList>
public class func fetchMomentListsWithSubtype(momentListSubtype: PHCollectionListSubtype, containingMoment moment: PHAssetCollection, options: PHFetchOptions?) -> PHFetchResult
    
/// 根据指定的中PHCollectionListSubtype类型获取PHCollectionList
///
/// - parameter momentListSubtype : PHCollectionListSubtype详细类型
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHCollectionList>
public class func fetchMomentListsWithSubtype(momentListSubtype: PHCollectionListSubtype, options: PHFetchOptions?) -> PHFetchResult
```

#2 获取PHCollectionList元数据

```swift
/// PHCollectionList类型
public var collectionListType: PHCollectionListType { get }
/// PHCollectionList详细类型
public var collectionListSubtype: PHCollectionListSubtype { get }
    
/// 开始时间
public var startDate: NSDate? { get }
/// 结束时间
public var endDate: NSDate? { get }

/// 照片的地址
public var localizedLocationNames: [String] { get }
```

#3 创建临时PHCollectionList

```swift
/// 通过[PHAsset]集合生成临时PHCollectionList
///
/// - parameter assets : [PHAsset]
/// - parameter title : 资产集合的名
///
/// - returns: PHCollectionList
public class func transientCollectionListWithCollections(collections: [PHCollection], title: String?) -> PHCollectionList
    
/// 通过PHFetchResult结果集生成临时PHCollectionList
///
/// - parameter fetchResult: PHFetchResult结果集
/// - parameter title : 资产集合的名
///
/// - returns: PHCollectionList
public class func transientCollectionListWithCollectionsFetchResult(fetchResult: PHFetchResult, title: String?) -> PHCollectionList
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHObject Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHObject_Class/index.html)

[PHCollection Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHCollection_Class/index.html)

[PHCollectionList Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHCollectionList_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-05 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog