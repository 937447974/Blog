NSMigrationManager是迁移管理器，通过它我们可以更精细的控制数据库迁移的工程，并能获取迁移的进度。

# Symbols

## Initializing a Manager

```swift
// 初始化
public init(sourceModel: NSManagedObjectModel, destinationModel: NSManagedObjectModel)
```

## Performing Migration Operations

```swift
// 执行迁移数据库
open func migrateStore(from sourceURL: URL, sourceType sStoreType: String, options sOptions: [AnyHashable : Any]? = nil, with mappings: NSMappingModel?, toDestinationURL dURL: URL, destinationType dStoreType: String, destinationOptions dOptions: [AnyHashable : Any]? = nil) throws
// 重置
open func reset()
// 通过错误取消迁移
open func cancelMigrationWithError(_ error: Error)
```

## Monitoring Migration Progress

```swift
// 迁移的进度
open var migrationProgress: Float { get }
// 当前正在处理的实体映射
open var currentEntityMapping: NSEntityMapping { get }
```

## Working with Source and Destination Instances

```swift
// 关联关系
open func associate(sourceInstance: NSManagedObject, withDestinationInstance destinationInstance: NSManagedObject, for entityMapping: NSEntityMapping)

// 获取源库中匹配的实体
open func destinationInstances(forEntityMappingName mappingName: String, sourceInstances: [NSManagedObject]?) -> [NSManagedObject]

// 获取目标库中匹配的实体
open func sourceInstances(forEntityMappingName mappingName: String, destinationInstances: [NSManagedObject]?) -> [NSManagedObject]
```

## Getting Information About a Migration Manager

```swift
// 映射模型
open var mappingModel: NSMappingModel { get }
// 源托管对象模型
open var sourceModel: NSManagedObjectModel { get }
// 目标托管对象模型
open var destinationModel: NSManagedObjectModel { get }
// 源托管对象上下文
open var sourceContext: NSManagedObjectContext { get }
// 目标托管对象上下文
open var destinationContext: NSManagedObjectContext { get }
// 通过实体映射，获取源实体描述
open func sourceEntity(for mEntity: NSEntityMapping) -> NSEntityDescription?
// 通过实体映射，获取目标库实体描述
open func destinationEntity(for mEntity: NSEntityMapping) -> NSEntityDescription?
// 携带的数据
open var userInfo: [AnyHashable : Any]?
```

## Using Store-Specific Migration Managers

```swift
// 是否使用特定的迁移管理器迁移
@available(iOS 5.0, *)
open var usesStoreSpecificMigrationManager: Bool
```

&#160;

----------

# Appendix

## Related Documentation

[NSMigrationManager](https://developer.apple.com/reference/coredata/nsmigrationmanager)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974