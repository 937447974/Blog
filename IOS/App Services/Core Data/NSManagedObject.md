NSManagedObject即数据库中的存放每一行数据，多数情况下我们是创建它的子类使用。

#Symbols

##1 Initializing a Managed Object

```swift
// 初始化
public init(entity: NSEntityDescription, insertInto context: NSManagedObjectContext?)
```

##2 Getting a Managed Object’s Identity

```swift
// 实体描述
open var entity: NSEntityDescription { get }
// 对象ID
open var objectID: NSManagedObjectID { get }
```

##3 Getting State Information

```swift
// 获取托管对象上下文
unowned(unsafe) open var managedObjectContext: NSManagedObjectContext? { get }
// 是否有变动
@available(iOS 5.0, *)
open var hasChanges: Bool { get }
// 是否插入
open var isInserted: Bool { get }
// 是否更新
open var isUpdated: Bool { get }
// 是否删除
open var isDeleted: Bool { get }
// 是否已脱离托管对象上下文
open var isFault: Bool { get }
// fault状态
@available(iOS 3.0, *)
open var faultingState: Int { get }
// 是否脱离关系
@available(iOS 3.0, *)
open func hasFault(forRelationshipNamed key: String) -> Bool
```

##4 Managing Life Cycle and Change Events

```swift
// 从查询中创建
open func awakeFromFetch()
// 从插入数据库中创建
open func awakeFromInsert()
// Callback for undo, redo, and other multi-property state resets 
@available(iOS 3.0, *)
open func awake(fromSnapshotEvents flags: NSSnapshotEventType)
// 值将发生改变
open func changedValues() -> [String : Any]
// 值已发生改变
@available(iOS 5.0, *)
open func changedValuesForCurrentEvent() -> [String : Any]
// 提交需要改变的字段
open func committedValues(forKeys keys: [String]?) -> [String : Any]
// 将被删除
@available(iOS 3.0, *)
open func prepareForDeletion()
// 将被保存
open func willSave()
// 已经保存
open func didSave()
// 将脱离管理
@available(iOS 3.0, *)
open func willTurnIntoFault()
// 已经脱离管理
open func didTurnIntoFault()
```

##5 Supporting Key-Value Coding

```swift
// 获取值
open func value(forKey key: String) -> Any?
// 设置值
open func setValue(_ value: Any?, forKey key: String)    
// 获取私有存储
open func primitiveValue(forKey key: String) -> Any?
// 设置私有存储
open func setPrimitiveValue(_ value: Any?, forKey key: String)
```

##6 Validation

```swift
// 校验数据
open func validateValue(_ value: AutoreleasingUnsafeMutablePointer<AnyObject?>, forKey key: String) throws
// 删除前校验
open func validateForDelete() throws
// 插入前校验
open func validateForInsert() throws
// 更新前校验
open func validateForUpdate() throws
```

##7 Supporting Key-Value Observing

```swift
// 键值对观察支持
open func willAccessValue(forKey key: String?) // read notification
open func didAccessValue(forKey key: String?) 
// 值改变
open func willChangeValue(forKey key: String)
open func didChangeValue(forKey key: String)
// 值改变，并携带参数
open func willChangeValue(forKey inKey: String, withSetMutation inMutationKind: NSKeyValueSetMutationKind, using inObjects: Set<AnyHashable>)
open func didChangeValue(forKey inKey: String, withSetMutation inMutationKind: NSKeyValueSetMutationKind, using inObjects: Set<AnyHashable>)
```

##8 Initializers

```swift
// 初始化
@available(iOS 10.0, *)
public convenience init(context moc: NSManagedObjectContext)
```

##9 Instance Properties

```swift
// 是否瞬态变化
@available(iOS 7.0, *)
open var hasPersistentChangedValues: Bool { get }
```

##10 Type Properties

```swift
// Distinguish between changes that should and should not dirty the object for any key unknown to Core Data.
@available(iOS 3.0, *)
open class var contextShouldIgnoreUnmodeledPropertyChanges: Bool { get }
```

##11 Instance Methods

```swift
// 批量获取有关系的对象ID
@available(iOS 8.3, *)
open func objectIDs(forRelationshipNamed key: String) -> [NSManagedObjectID]
```

##12 Type Methods

```swift
// 实体描述
@available(iOS 10.0, *)
open class func entity() -> NSEntityDescription
// 快速创建查询请求
@available(iOS 10.0, *)
open class func fetchRequest() -> NSFetchRequest<NSFetchRequestResult>
```

&#160;

----------

#Appendix

##Related Documentation

[NSManagedObject](https://developer.apple.com/reference/coredata/nsmanagedobject)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-26 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974