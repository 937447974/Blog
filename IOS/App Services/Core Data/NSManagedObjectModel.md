NSManagedObjectModel是托管对象模型，标示着Core Data对应的数据实体。

托管对象模型包含一个或多个NSEntityDescription对象，NSEntityDescription记录的就是实体的描述信息。


## 1 Initializing a Model

```swift
// 通过指定的url地址生成模型
public convenience init?(contentsOf url: URL)
// 返回合并所有在给定束中发现的模型创建的模型。
open class func mergedModel(from bundles: [Bundle]?) -> NSManagedObjectModel? 
// 从指定的元数据返回一个指定数组的合并模型，以提供版本信息的合并模型。
@available(iOS 3.0, *)
open class func mergedModel(from bundles: [Bundle]?, forStoreMetadata metadata: [String : Any]) -> NSManagedObjectModel?
// 从现有的数据模型合并成一个数据模型
public /*not inherited*/ init?(byMerging models: [NSManagedObjectModel]?)
// 通过指定的版本信息合并数据模型
@available(iOS 3.0, *)
    public /*not inherited*/ init?(byMerging models: [NSManagedObjectModel], forStoreMetadata metadata: [String : Any])
```

## 2 Entities and Configurations

```swift
// 实体字典，key为实体名
open var entitiesByName: [String : NSEntityDescription] { get }
// 实体数组
open var entities: [NSEntityDescription]
// 可用的配置名称
open var configurations: [String] { get }
// 通过配置名获取实体
open func entities(forConfigurationName configuration: String?) -> [NSEntityDescription]?
// 将指定配置名和实体集合关联
open func setEntities(_ entities: [NSEntityDescription], forConfigurationName configuration: String)
```

## 3 Getting Fetch Request Templates

```swift
// 获取所有快速查询模板
@available(iOS 3.0, *)
open var fetchRequestTemplatesByName: [String : NSFetchRequest<NSFetchRequestResult>] { get }
// 设置查询模板
open func setFetchRequestTemplate(_ fetchRequestTemplate: NSFetchRequest<NSFetchRequestResult>?, forName name: String)    
// 根据关键字获取模板
open func fetchRequestTemplate(forName name: String) -> NSFetchRequest<NSFetchRequestResult>?    
// 根据关键字获取，并可用字典替换
open func fetchRequestFromTemplate(withName name: String, substitutionVariables variables: [String : Any]) -> NSFetchRequest<NSFetchRequestResult>?
    
```

## 4 Localization

```swift
// 定位字典
open var localizationDictionary: [String : String]?
```

## 5 Versioning and Migration

```swift
// 开发人员定义的版本标识符
@available(iOS 3.0, *)
open var versionIdentifiers: Set<AnyHashable>    
// 判断源是否兼容，主要用于判断是否需要做数据库迁移
@available(iOS 3.0, *)
open func isConfiguration(withName configuration: String?, compatibleWithStoreMetadata metadata: [String : Any]) -> Bool    
// 版本兼容性相关信息
@available(iOS 3.0, *)
open var entityVersionHashesByName: [String : Data] { get }
```

## 6 Initializers

```swift
// 初始化
public init()
```

&#160;

----------

# Appendix

## Related Documentation

[NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-22 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974