PHAssetCollection是PHCollection的子类，代表照片或视频的集合，可以理解为系统“照片”App里面的相册或者其重要时刻。

# 1 获取资产集合

```swift
/// 通过唯一标示符获取资产集合
///
/// - parameter identifiers : 对应PHAsset的localIdentifier属性
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAssetCollection>
public class func fetchAssetCollectionsWithLocalIdentifiers(identifiers: [String], options: PHFetchOptions?) -> PHFetchResult
    
/// 通过资产类型获取资产集合
///
/// - parameter type : PHAssetCollectionType 资产集合的类型
/// - parameter subtype : PHAssetCollectionSubtype 资产集合的具体类型
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAssetCollection>
public class func fetchAssetCollectionsWithType(type: PHAssetCollectionType, subtype: PHAssetCollectionSubtype, options: PHFetchOptions?) -> PHFetchResult
    
// Smart Albums are not supported, only Albums and Moments
/// 通过PHAsset获取其资产集合，只支持Albums和Moments
///
/// - parameter asset : PHAsset 资产
/// - parameter type : PHAssetCollectionType 资产集合的具体类型
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAssetCollection>
public class func fetchAssetCollectionsContainingAsset(asset: PHAsset, withType type: PHAssetCollectionType, options: PHFetchOptions?) -> PHFetchResult
    
/// 通过ALAssetGroup的ALAssetsGroupPropertyURL获取资产集合
///
/// - parameter assetGroupURLs : [NSURL]对应ALAssetGroup's ALAssetsGroupPropertyURL
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAssetCollection>
public class func fetchAssetCollectionsWithALAssetGroupURLs(assetGroupURLs: [NSURL], options: PHFetchOptions?) -> PHFetchResult
    
/// 通过PHCollectionList获取其对应的重要时刻的资产集合
///
/// - parameter momentList : PHCollectionList
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAssetCollection>
public class func fetchMomentsInMomentList(momentList: PHCollectionList, options: PHFetchOptions?) -> PHFetchResult
    
/// 获取所有重要时刻的资产集合
///
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAssetCollection>
public class func fetchMomentsWithOptions(options: PHFetchOptions?) -> PHFetchResult
```

# 2 获取集合元数据

```swift
/// 集合类型
public var assetCollectionType: PHAssetCollectionType { get }
/// 集合详细类型
public var assetCollectionSubtype: PHAssetCollectionSubtype { get }
    
/// 内部资产数量
public var estimatedAssetCount: Int { get }

/// 开始时间  
public var startDate: NSDate? { get }
/// 结束时间
public var endDate: NSDate? { get }

/// 定位的位置
public var approximateLocation: CLLocation? { get }
/// 定位的位置名称
public var localizedLocationNames: [String] { get }
```

# 3 创建临时资产集合

```swift
/// 通过[PHAsset]集合生成临时PHAssetCollection
///
/// - parameter assets : [PHAsset]
/// - parameter title : 资产集合的名
///
/// - returns: PHAssetCollection
public class func transientAssetCollectionWithAssets(assets: [PHAsset], title: String?) -> PHAssetCollection
    
/// 通过PHFetchResult结果集生成临时PHAssetCollection
///
/// - parameter fetchResult: PHFetchResult
/// - parameter title : 资产集合的名
///
/// - returns: PHAssetCollection
public class func transientAssetCollectionWithAssetFetchResult(fetchResult: PHFetchResult, title: String?) -> PHAssetCollection
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

[PHAssetCollection Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHAssetCollection_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-05 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog