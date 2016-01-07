[Photos(PHAssetChangeRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHAssetChangeRequest).md)

[Photos(PHAssetCreationRequest)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHAssetCreationRequest).md)

----

PHAssetCreationRequest是PHAssetChangeRequest的子类，继承了PHAssetChangeRequest的所有属性和方法。通过它你可以更精细的创建PHAsset，如通过NSSata数据。

#1 Requesting Asset Creation

```swift
/// 初始化
public class func creationRequestForAsset() -> Self
```

#2 Preflighting a Request

```swift
/// 判断能否创建指定资源类型的资产
///
/// - parameter types : [NSNumber]即[PHAssetResourceType]
///
/// - returns: Bool
public class func supportsAssetResourceTypes(types: [NSNumber]) -> Bool
```

#3 Providing Data Resources for the New Asset

```swift
/// 通过文件增加
///
/// - parameter type : PHAssetResourceType
/// - parameter fileURL : NSURL
/// - parameter options : PHAssetResourceCreationOptions
///
/// - returns: void
public func addResourceWithType(type: PHAssetResourceType, fileURL: NSURL, options: PHAssetResourceCreationOptions?)
    
/// 通过NSData增加
///
/// - parameter type : PHAssetResourceType
/// - parameter data : NSData添加的数据
/// - parameter options : PHAssetResourceCreationOptions
///
/// - returns: void
public func addResourceWithType(type: PHAssetResourceType, data: NSData, options: PHAssetResourceCreationOptions?)
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