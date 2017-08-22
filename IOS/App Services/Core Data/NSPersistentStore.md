NSPersistentStore是持久化存储核心数据的抽象类，可以理解为持久化存储区。持久化存储区支持的数据类型为SQLite、二进制、XML和内存。

# Symbols

## 1 Initializing a Persistent Store

```swift
// 初始化
public init(persistentStoreCoordinator root: NSPersistentStoreCoordinator?, configurationName name: String?, at url: URL, options: [AnyHashable : Any]? = nil)
```

## 2 Working with State Information

```swift
// 类型
open var type: String { get }
// 持久化存储协调器
weak open var persistentStoreCoordinator: NSPersistentStoreCoordinator? { get } 
// 配置名称
open var configurationName: String { get }
// 初始化的可选字典
open var options: [AnyHashable : Any]? { get }
// 持久化存储区的地址
open var url: URL?
// 唯一标识符
open var identifier: String!
// 是否只读
open var isReadOnly: Bool
```

## 3 Managing Metadata

```swift
// 根据地址获取持久化存储区的元数据
open class func metadataForPersistentStore(with url: URL) throws -> [String : Any]
// 设置元数据到指定的地址
open class func setMetadata(_ metadata: [String : Any]?, forPersistentStoreAt url: URL) throws
// 元数据
open var metadata: [String : Any]!
// 加载元数据
open var metadata: [String : Any]!
```

## 4 Setup and Teardown

```swift
// 关联到持久化存储协调器
open func didAdd(to coordinator: NSPersistentStoreCoordinator)
// 从持久化存储协调器中移除
open func willRemove(from coordinator: NSPersistentStoreCoordinator?)
```

## 5 Supporting Migration

```swift
// 获取商店的迁移管理器类
@available(iOS 3.0, *)
open class func migrationManagerClass() -> Swift.AnyClass
```

&#160;

----------

# Appendix

## Related Documentation

[NSPersistentStore](https://developer.apple.com/reference/coredata/nspersistentstore)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-24 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974