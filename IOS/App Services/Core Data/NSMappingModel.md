NSMappingModel是映射模型，用于比较两个托管对象模型之间的差异。即使用Xcode创建的如下所示的文件。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016102601.png)

# Symbols

## 1 Creating a Mapping

```swift
// 通过托管对象模型初始化
public /*not inherited*/ init?(from bundles: [Bundle]?, forSourceModel sourceModel: NSManagedObjectModel?, destinationModel: NSManagedObjectModel?)
// 通过托管对象模型初始化
@available(iOS 3.0, *)
open class func inferredMappingModel(forSourceModel sourceModel: NSManagedObjectModel, destinationModel: NSManagedObjectModel) throws -> NSMappingModel
// 通过文件地址初始化
public init?(contentsOf url: URL?)
```

## 2 Managing Entity Mappings

```swift
// 映射数组
open var entityMappings: [NSEntityMapping]!
// 映射字典
open var entityMappingsByName: [String : NSEntityMapping] { get }
```

&#160;

----------

# Appendix

## Related Documentation

[NSMappingModel](https://developer.apple.com/reference/coredata/nsmappingmodel)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974