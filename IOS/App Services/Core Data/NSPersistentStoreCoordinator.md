NSPersistentStoreCoordinator是持久化存储协调者，主要用于协调托管对象上下文和持久化存储区之间的关系。NSManagedObjectContext使用协调者的托管对象模型将数据保存到数据库，或查询数据。


# Symbols

## 1 Registered Store Types

```swift
// 对于存储的类型注册指定的NSPersistentStore子类
@available(iOS 3.0, *)
open class func registerStoreClass(_ storeClass: Swift.AnyClass, forStoreType storeType: String)
```

## 2 Initializing a Coordinator

```swift
// 通过托管对象模型初始化
public init(managedObjectModel model: NSManagedObjectModel)
// 托管对象模型
open var managedObjectModel: NSManagedObjectModel { get }
```

## 3 Configuring Persistent Stores

```swift
// 通过指定位置生成一个持久性存储库
open func addPersistentStore(ofType storeType: String, configurationName configuration: String?, at storeURL: URL?, options: [AnyHashable : Any]? = nil) throws -> NSPersistentStore
// 对于持久性存储设置地址
@available(iOS 3.0, *)
open func setURL(_ url: URL, for store: NSPersistentStore) -> Bool
// 移除持久性存储
open func remove(_ store: NSPersistentStore) throws
// 一定持久性存储到指定位置
open func migratePersistentStore(_ store: NSPersistentStore, to URL: URL, options: [AnyHashable : Any]? = nil, withType storeType: String) throws -> NSPersistentStore
// 获取所有持久性存储
open var persistentStores: [NSPersistentStore] { get }
// 通过地址获取持久性存储
open func persistentStore(for URL: URL) -> NSPersistentStore?
// 通过持久性存储获取其存放地址
open func url(for store: NSPersistentStore) -> URL
```

## 4 Executing a Fetch Request

```swift
// 将持久化存储关联到托管对象上下文
 @available(iOS 5.0, *)
open func execute(_ request: NSPersistentStoreRequest, with context: NSManagedObjectContext) throws -> Any
```

## 5 Working with Metadata

```swift
// 获取持久化存储的元数据
open func metadata(for store: NSPersistentStore) -> [String : Any]
// 设置持久化存储的元数据
open func setMetadata(_ metadata: [String : Any]?, for store: NSPersistentStore)
```

## 6 Discovering Object IDs

```swift
// 获取匹配的持久化存储ID
open func managedObjectID(forURIRepresentation url: URL) -> NSManagedObjectID?
```

&#160;

----------

# Appendix

## Related Documentation

[NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-24 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974