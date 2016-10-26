NSManagedObjectContext是托管对象上下文，其中包含多个托管对象。托管对象上下文负责管理其中对象的生命周期，并且负责提供许多强大的功能，诸如faulting、变更追踪、验证等。faulting就是用户从持久化存储区中获取数据时，系统只会把需要用到的那一部分获取过来。变更追踪用户支持撤销及重做功能。验证机制用来确保由托管对象模型所订立的规则。

持久化存储区可以有很多个，与之类似，托管对象上下文也可以不止一个。

#Notifications

我们也可以通过通知监听托管对象上下文的变化状态。

```objc
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(<#Selector name#>) name: NSManagedObjectContextObjectsDidChange object:<#A managed object context#>];
```

还有其他通知字段，如NSManagedObjectContextDidSave和NSManagedObjectContextWillSave

#Concurrency

Core Data使用专有的队列来保护管理对象。其中有三种方式可供选择。

1. Confinement (confinementConcurrencyType)：每个线程一个独立的Context，主要是为了兼容之前的设计。
2. Private queue (privateQueueConcurrencyType)：私有队列，不会阻塞主线程。
3. Main queue (mainQueueConcurrencyType)：主线程，会阻塞。

> 其中confinementConcurrencyType已被弃用。

#Symbols

##1 Registering and Fetching Objects

```swift
// 获取请求的数据
open func fetch(_ request: NSFetchRequest<NSFetchRequestResult>) throws -> [Any]
// 请求的数据的个数
open func count(for request: NSFetchRequest<NSFetchRequestResult>) throws -> Int
// 通过NSManagedObjectID生成实体
open func registeredObject(for objectID: NSManagedObjectID) -> NSManagedObject?
// 通过NSManagedObjectID获取实体
open func object(with objectID: NSManagedObjectID) -> NSManagedObject
// 通过NSManagedObjectID获取实体，可以抛出错误
@available(iOS 3.0, *)
open func existingObject(with objectID: NSManagedObjectID) throws -> NSManagedObject
// 返回注册的对象组
open var registeredObjects: Set<NSManagedObject> { get }
```

## Managed Object Management

```swift
// 插入一个实体
open func insert(_ object: NSManagedObject)
// 删除一个实体
open func delete(_ object: NSManagedObject)
// 指定对象保存到数据库
open func assign(_ object: Any, to store: NSPersistentStore)
// 转化永久ID
@available(iOS 3.0, *)
open func obtainPermanentIDs(for objects: [NSManagedObject]) throws
// 对象的冲突检查
open func detectConflicts(for object: NSManagedObject)
// 释放对象，mergeChanges是否合并到数据库
open func refresh(_ object: NSManagedObject, mergeChanges flag: Bool)
// 通知合并对象
open func processPendingChanges()
// 批量插入NSManagedObject
open var insertedObjects: Set<NSManagedObject> { get }
// 批量更新NSManagedObject
open var updatedObjects: Set<NSManagedObject> { get }
// 批量删除NSManagedObject
open var deletedObjects: Set<NSManagedObject> { get }
```

## Managing Concurrency

```swift
// 初始化
@available(iOS 5.0, *)
public init(concurrencyType ct: NSManagedObjectContextConcurrencyType)
// 变化类型
@available(iOS 5.0, *)
open var concurrencyType: NSManagedObjectContextConcurrencyType { get }
```

## Undo Management

```swift
// 撤销管理器
open var undoManager: UndoManager?
// 撤销
open func undo()
// 重做
open func redo()
// 重置
open func reset()
// 回滚
open func rollback()
// 保存
open func save() throws
// 是否有变化
open var hasChanges: Bool { get }
```

## Managing the Parent Store

```swift
// 持久化化存储协调者
open var persistentStoreCoordinator: NSPersistentStoreCoordinator?
// 父托管对象上下文
@available(iOS 5.0, *)
open var parent: NSManagedObjectContext?
```

## Delete Propagation

```swift
// 是否传播删除事件
open var propagatesDeletesAtEndOfEvent: Bool
```

## Registered Objects

```swift
// 是否强引用注册对象
open var retainsRegisteredObjects: Bool
```

## Managing the Staleness Interval

```swift
// 数据缓存的时间
open var stalenessInterval: TimeInterval
```

## Managing the Merge Policy

```swift
// 合并方式
open var mergePolicy: Any// default: NSErrorMergePolicy
```

## Performing Block Operations

```swift
// 异步执行block
@available(iOS 5.0, *)
open func perform(_ block: @escaping () -> Swift.Void)
// 同步执行block
@available(iOS 5.0, *)
open func performAndWait(_ block: @escaping () -> Swift.Void)
```

## User Info

```swift
// 携带的数据
@available(iOS 5.0, *)
open var userInfo: NSMutableDictionary { get }
```

## Instance Properties

```swift
// 是否自动保存到父队列
@available(iOS 10.0, *)
open var automaticallyMergesChangesFromParent: Bool
// 名称
@available(iOS 8.0, *)
open var name: String?
// 上下文的token
@available(iOS 10.0, *)
open var queryGenerationToken: NSQueryGenerationToken? { get }
// 删除是否抛错
@available(iOS 9.0, *)
open var shouldDeleteInaccessibleFaults: Bool
```

## Instance Methods

```swift
// 释放所有对象
@available(iOS 8.3, *)
open func refreshAllObjects()
```

## Type Methods

```swift
// 手动发出保存的通知
@available(iOS 3.0, *)
open func mergeChanges(fromContextDidSave notification: Notification)
// 发出保存的通知并携带变动的上下文
@available(iOS 9.0, *)
open class func mergeChanges(fromRemoteContextSave changeNotificationData: [AnyHashable : Any], into contexts: [NSManagedObjectContext])
```

&#160;

----------

#Appendix

##Related Documentation

[NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-25 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974