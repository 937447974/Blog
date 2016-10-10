[Photos(PHChange)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHChange).md)

[Photos(PHObjectChangeDetails)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHObjectChangeDetails).md)

[Photos(PHFetchResultChangeDetails)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHFetchResultChangeDetails).md)

---

PHFetchResultChangeDetails主要用于记录PHFetchResult的变动，这样可以同时判断多个数据是否有变动。

#1 获取PHFetchResult

```swift
/// 变化前
public var fetchResultBeforeChanges: PHFetchResult { get }
    
/// 变化后
public var fetchResultAfterChanges: PHFetchResult { get }
``` 

#2 获取变动信息

```swift
/// 是否有变动
public var hasIncrementalChanges: Bool { get }
    
/// 删除对象的位置
public var removedIndexes: NSIndexSet? { get }
/// 删除的对象
public var removedObjects: [PHObject] { get }
    
/// 插入对象的位置
public var insertedIndexes: NSIndexSet? { get }
/// 插入的对象
public var insertedObjects: [PHObject] { get }
    
/// 有变化的对象位置
public var changedIndexes: NSIndexSet? { get }
/// 有变化的对象
public var changedObjects: [PHObject] { get }
    
/// 移动照片
public func enumerateMovesWithBlock(handler: (Int, Int) -> Void)

/// 是否有移动
public var hasMoves: Bool { get }
```

#3 比较PHFetchResult

```swift
/// 获取两个PHFetchResult的差异
///
/// - parameter fromResult : PHFetchResult
/// - parameter toResult : PHFetchResult
/// - parameter changedObjects : [PHObject]
///
/// - returns: PHFetchResultChangeDetails
public convenience init(fromFetchResult fromResult: PHFetchResult, toFetchResult toResult: PHFetchResult, changedObjects: [PHObject])
```

#4 实战演练

```swift
func photoLibraryDidChange(changeInfo: PHChange!) {
    
    // Photos may call this method on a background queue;
    // switch to the main queue to update the UI.
    dispatch_async(dispatch_get_main_queue()) {
        
        // Check for changes to the displayed album itself
        // (its existence and metadata, not its member assets).
        if let albumChanges = changeInfo.changeDetailsForObject(self.displayedAlbum) {
            // Fetch the new album and update the UI accordingly.
            self.displayedAlbum = albumChanges.objectAfterChanges as PHAssetCollection
            self.navigationController.navigationItem.title = self.displayedAlbum.localizedTitle
        }
        
        // Check for changes to the list of assets (insertions, deletions, moves, or updates).
        if let collectionChanges = changeInfo.changeDetailsForFetchResult(self.assetsFetchResults) {
            
            // Get the new fetch result for future change tracking.
            self.assetsFetchResults = collectionChanges.fetchResultAfterChanges
            
            if collectionChanges.hasIncrementalChanges {
                
                // Get the changes as lists of index paths for updating the UI.
                var removedPaths: [NSIndexPath]?
                var insertedPaths: [NSIndexPath]?
                var changedPaths: [NSIndexPath]?
                if let removed = collectionChanges.removedIndexes {
                    removedPaths = self.indexPathsFromIndexSet(removed)
                }
                if let inserted = collectionChanges.insertedIndexes {
                    insertedPaths = self.indexPathsFromIndexSet(inserted)
                }
                if let changed = collectionChanges.changedIndexes {
                    changedPaths = self.indexPathsFromIndexSet(changed)
                }
                
                // Tell the collection view to animate insertions/deletions/moves
                // and to refresh any cells that have changed content.
                self.collectionView.performBatchUpdates(
                    {
                        if removedPaths {
                            self.collectionView.deleteItemsAtIndexPaths(removedPaths)
                        }
                        if insertedPaths {
                            self.collectionView.insertItemsAtIndexPaths(insertedPaths)
                        }
                        if changedPaths {
                            self.collectionView.reloadItemsAtIndexPaths(changedPaths)
                        }
                        if (collectionChanges.hasMoves) {
                            collectionChanges.enumerateMovesWithBlock() { fromIndex, toIndex in
                                let fromIndexPath = NSIndexPath(forItem: fromIndex, inSection: 0)
                                let toIndexPath = NSIndexPath(forItem: toIndex, inSection: 0)
                                self.collectionView.moveItemAtIndexPath(fromIndexPath, toIndexPath: toIndexPath)
                            }
                        }
                    }, completion: nil)
            } else {
                // Detailed change information is not available;
                // repopulate the UI from the current fetch result.
                self.collectionView.reloadData()
            }
        }
    }
}
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHChange Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHChange_Class/index.html)

[PHFetchResultChangeDetails Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHFetchResultChangeDetails_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-06 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog