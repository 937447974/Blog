NSFetchRequest即SQL的select操作。

# Symbols
## 1 Managing the Fetch Request’s Entity

```swift
// 通过实体名初始化
@available(iOS 4.0, *)
public convenience init(entityName: String)
// 实体描述
open var entity: NSEntityDescription?
// 是否包含子实体的结果
@available(iOS 3.0, *)
open var includesSubentities: Bool
```

## 2 Fetch Constraints

```swift
// 谓词，即where
open var predicate: NSPredicate?
// 返回数据的上限
open var fetchLimit: Int
// 返回数据的偏移量，即跳过前面多少数据
@available(iOS 3.0, *)
open var fetchOffset: Int
// 分页获取
@available(iOS 3.0, *)
open var fetchBatchSize: Int
// 指定的持久化存储
open var affectedStores: [NSPersistentStore]?
```

## 3 Sorting

```swift
// 排序集合
open var sortDescriptors: [NSSortDescriptor]?
```

## 4 Prefetching

```swift
// 预获取关系数据
@available(iOS 3.0, *)
open var relationshipKeyPathsForPrefetching: [String]?
```

## 5 Managing How Results Are Returned

```swift
// 返回的类型
@available(iOS 3.0, *)
open var resultType: NSFetchRequestResultType
// 是否从内存中获取数据
@available(iOS 3.0, *)
open var includesPendingChanges: Bool
// 指定获取的属性
@available(iOS 3.0, *)
open var propertiesToFetch: [Any]?
// 是否去重
@available(iOS 3.0, *)
open var returnsDistinctResults: Bool
// 获取的数据是否缓存
@available(iOS 3.0, *)
open var includesPropertyValues: Bool
// 是否用获取的数据刷新当前缓存
@available(iOS 5.0, *)
open var shouldRefreshRefetchedObjects: Bool
// 返回的子实体是否加载到内存中
@available(iOS 3.0, *)
open var returnsObjectsAsFaults: Bool
```

## 6 Grouping and Filtering Dictionary Results

```swift
// GROUP BY操作
@available(iOS 5.0, *)
open var propertiesToGroupBy: [Any]?
// 分组
@available(iOS 5.0, *)
open var havingPredicate: NSPredicate?
```

## 7 Initializers

```swift
// 初始化
public init()
```

## 8 Instance Methods

```swift
// 提交请求
@available(iOS 10.0, *)
open func execute() throws -> [ResultType]
```

&#160;

----------

# Appendix

## Related Documentation

[NSFetchRequest](https://developer.apple.com/reference/coredata/nsfetchrequest)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974