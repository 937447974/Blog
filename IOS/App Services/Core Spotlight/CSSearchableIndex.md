**1 [CSSearchableIndex](#1)**

1. [Determining Indexing Capability](#1.1)
2. [Getting an Index](#1.2)
3. [Setting the Delegate](#1.3)
4. [Managing Items in an Index](#1.4)
5. [Batching Index Updates](#1.5)

**2 [CSSearchableIndexDelegate](#2)**

1. [Updating the Index](#2.1)

----

#1 CSSearchableIndex

CSSearchableIndex管理Spotlight搜索栏的增删改查。

##1.1 Determining Indexing Capability

```swift
// 判断能否支持管理搜索栏
public class func isIndexingAvailable() -> Bool
```

##1.2 Getting an Index

```swift
// 获取默认CSSearchableIndex
public class func defaultSearchableIndex() -> Self
    
/// 初始化CSSearchableIndex
///
/// - parameter name : 自定义名
///
/// - returns: CSSearchableIndex
public init(name: String)
    
/// 初始化CSSearchableIndex
///
/// - parameter name : 自定义名
/// - parameter protectionClass : The file protection class. Acceptable values are NSFileProtectionNone, NSFileProtectionComplete, NSFileProtectionCompleteUnlessOpen, or NSFileProtectionCompleteUntilFirstUserAuthentication.
///
/// - returns: CSSearchableIndex
public init(name: String, protectionClass: String?)
```

##1.3 Setting the Delegate

```swift
// CSSearchableIndexDelegate代理
weak public var indexDelegate: CSSearchableIndexDelegate?
```

##1.4 Managing Items in an Index

```swift
// 增加、更新CSSearchableItem
public func indexSearchableItems(items: [CSSearchableItem], completionHandler: ((NSError?) -> Void)?)
    
// 删除CSSearchableItem
public func deleteSearchableItemsWithIdentifiers(identifiers: [String], completionHandler: ((NSError?) -> Void)?)

// 根据CSSearchableItem.domainIdentifier删除CSSearchableItem
public func deleteSearchableItemsWithDomainIdentifiers(domainIdentifiers: [String], completionHandler: ((NSError?) -> Void)?)
    
// 删除所有CSSearchableItem
public func deleteAllSearchableItemsWithCompletionHandler(completionHandler: ((NSError?) -> Void)?)
```

##1.5 Batching Index Updates

```swift
// 开始批量更新CSSearchableItem
public func beginIndexBatch()
    
// 结束批量更新CSSearchableItem
public func endIndexBatchWithClientState(clientState: NSData, completionHandler: ((NSError?) -> Void)?)
    
// 获取应用最新的存储状态信息
public func fetchLastClientStateWithCompletionHandler(completionHandler: (NSData?, NSError?) -> Void)
```
#2 CSSearchableIndexDelegate

CSSearchableIndex的代理，监听索引数据。

##2.1 Updating the Index

```swift
// 索引请求这个代理重新索引所有可搜索的数据，并且清除任何本地状态（可能该状态已经被持久化），因为索引已经丢失了。
public func searchableIndex(searchableIndex: CSSearchableIndex, reindexAllSearchableItemsWithAcknowledgementHandler acknowledgementHandler: () -> Void)

// 根据给定的identifiers重新索引可搜索的数据
public func searchableIndex(searchableIndex: CSSearchableIndex, reindexSearchableItemsWithIdentifiers identifiers: [String], acknowledgementHandler: () -> Void)

// 为节约耗电，索引重组
optional public func searchableIndexDidThrottle(searchableIndex: CSSearchableIndex)

// 为节约耗电，索引重组结束
optional public func searchableIndexDidFinishThrottle(searchableIndex: CSSearchableIndex)
```

&#160;

----

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Core Spotlight Framework Reference](https://developer.apple.com/library/ios/documentation/CoreSpotlight/Reference/CoreSpotlight_Framework/index.html)

[App Search Programming Guide](https://developer.apple.com/library/ios/documentation/General/Conceptual/AppSearch/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-28 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog