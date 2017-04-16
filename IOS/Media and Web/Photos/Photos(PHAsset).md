每个PHAsset对象代表一个照片或视频。我们可以通过它获得照片或视频的相关属性。PHAsset是PHObject的子类。

# 1 获取PHAsset

```swift
/// 从PHAssetCollection中获取PHAsset
///
/// - parameter assetCollection : PHAssetCollection
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAsset>
public class func fetchAssetsInAssetCollection(assetCollection: PHAssetCollection, options: PHFetchOptions?) -> PHFetchResult
    
/// 通过localIdentifier获取PHAsset
///
/// - parameter identifiers : 对应PHAsset的localIdentifier属性
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAsset>
public class func fetchAssetsWithLocalIdentifiers(identifiers: [String], options: PHFetchOptions?) -> PHFetchResult
    
/// 从PHAssetCollection中获取PHAsset
///
/// - parameter assetCollection : PHAssetCollection
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAsset>
public class func fetchKeyAssetsInAssetCollection(assetCollection: PHAssetCollection, options: PHFetchOptions?) -> PHFetchResult?
    
/// 通过PHAsset的burstIdentifier属性获取PHAsset
///
/// - parameter burstIdentifier : 对应PHAsset的burstIdentifier属性
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAsset>
public class func fetchAssetsWithBurstIdentifier(burstIdentifier: String, options: PHFetchOptions?) -> PHFetchResult
    
/// 从所有PHAsset中筛选PHAsset
///
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAsset>
public class func fetchAssetsWithOptions(options: PHFetchOptions?) -> PHFetchResult
    
/// 通过类型获取PHAsset
///
/// - parameter mediaType : PHAssetMediaType
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAsset>
public class func fetchAssetsWithMediaType(mediaType: PHAssetMediaType, options: PHFetchOptions?) -> PHFetchResult

/// 通过ALAsset的ALAssetPropertyAssetURL地址获取PHAsset
///
/// - parameter assetURLs : [NSURL]
/// - parameter options : PHFetchOptions
///
/// - returns: PHFetchResult<PHAsset>
public class func fetchAssetsWithALAssetURLs(assetURLs: [NSURL], options: PHFetchOptions?) -> PHFetchResult
```

# 2 获取资产元数据

```swift
/// 资产类型
public var mediaType: PHAssetMediaType { get }
/// 资产类型细分
public var mediaSubtypes: PHAssetMediaSubtype { get }
/// 资产来源，如icloud、用户创建
@available(iOS 9.0, *)
public var sourceType: PHAssetSourceType { get }

/// 像素宽
public var pixelWidth: Int { get }
/// 像素高
public var pixelHeight: Int { get }

/// 创建时间
public var creationDate: NSDate? { get }
/// 修改时间
public var modificationDate: NSDate? { get }

/// 地址
public var location: CLLocation? { get }

/// 视频播放时间(秒)
public var duration: NSTimeInterval { get }
    
/// 是否隐藏
public var hidden: Bool { get }

/// 是否收藏
public var favorite: Bool { get }
```

# 3 编辑资产

```swift
/// 根据PHAssetEditOperation指定的类型判断能否编辑
///
/// - parameter editOperation : PHAssetEditOperation{Delete删除、Content内容、Properties属性}
///
/// - returns: Bool
public func canPerformEditOperation(editOperation: PHAssetEditOperation) -> Bool
    
/// 编辑PHAsset
///
/// - parameter options : PHContentEditingInputRequestOptions
/// - parameter completionHandler : 编辑回调
///
/// - returns: PHContentEditingInputRequestID编辑的会话标示符
@available(iOS 8.0, *)
public func requestContentEditingInputWithOptions(options: PHContentEditingInputRequestOptions?, completionHandler: (PHContentEditingInput?, [NSObject : AnyObject]) -> Void) -> PHContentEditingInputRequestID
    
/// 取消编辑
///
/// - parameter requestID : PHContentEditingInputRequestID编辑的会话标示符
///
/// - returns: void
@available(iOS 8.0, *)
public func cancelContentEditingInputRequest(requestID: PHContentEditingInputRequestID)
```

# 4 连拍图片管理

```swift
/// 连拍的唯一标示
public var burstIdentifier: String? { get }
/// 类型
public var burstSelectionTypes: PHAssetBurstSelectionType { get }
/// 是否是连拍照片
public var representsBurst: Bool { get }
```

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHObject Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHObject_Class/index.html)

[PHAsset Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHAsset_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-05 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog