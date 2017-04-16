UICollectionView如图所示是一个容器控件。

![](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/Art/uicollectionview_callouts.png)

## 1 Initializing a Collection View

```swift
/// 初始化
public init(frame: CGRect, collectionViewLayout layout: UICollectionViewLayout)
```

## 2 Configuring the Collection View

```swift
// 管理管理
weak public var delegate: UICollectionViewDelegate?
// 数据源代理
weak public var dataSource: UICollectionViewDataSource?
// 背景View
public var backgroundView: UIView?
```

## 3 Creating Collection View Cells

```swift
// 注册通过class创建cell
public func registerClass(cellClass: AnyClass?, forCellWithReuseIdentifier identifier: String)
// 注册通过xib创建cell
public func registerNib(nib: UINib?, forCellWithReuseIdentifier identifier: String)
    
// 注册通过class创建组头和组尾
public func registerClass(viewClass: AnyClass?, forSupplementaryViewOfKind elementKind: String, withReuseIdentifier identifier: String)
// 注册通过xib创建组头和组尾
public func registerNib(nib: UINib?, forSupplementaryViewOfKind kind: String, withReuseIdentifier identifier: String)
    
// 缓存队列获取cell
public func dequeueReusableCellWithReuseIdentifier(identifier: String, forIndexPath indexPath: NSIndexPath) -> UICollectionViewCell
// 缓存队列获取组头部组尾
public func dequeueReusableSupplementaryViewOfKind(elementKind: String, withReuseIdentifier identifier: String, forIndexPath indexPath: NSIndexPath) -> UICollectionReusableView
```

## 4 Changing the Layout

```swift
// 界面item显示约束
public var collectionViewLayout: UICollectionViewLayout
// 动画设置UICollectionViewLayout
public func setCollectionViewLayout(layout: UICollectionViewLayout, animated: Bool)
// 动画设置UICollectionViewLayout且有回调
@available(iOS 7.0, *)
public func setCollectionViewLayout(layout: UICollectionViewLayout, animated: Bool, completion: ((Bool) -> Void)?)
    
// 使用一个中间过渡UI变化
@available(iOS 7.0, *)
public func startInteractiveTransitionToCollectionViewLayout(layout: UICollectionViewLayout, completion: UICollectionViewLayoutInteractiveTransitionCompletion?) -> UICollectionViewTransitionLayout
// 完成过渡
@available(iOS 7.0, *)
public func finishInteractiveTransition()
// 取消过渡
@available(iOS 7.0, *)
public func cancelInteractiveTransition()
```

## 5 Reloading Content

```swift
// 刷新所有显item
public func reloadData()
// 刷新组
public func reloadSections(sections: NSIndexSet)
// 刷新多个cell
public func reloadItemsAtIndexPaths(indexPaths: [NSIndexPath])
```

## 6 Getting the State of the Collection View

```swift
// 显示多少组
public func numberOfSections() -> Int
// 每一组显示的item数量
public func numberOfItemsInSection(section: Int) -> Int
// 当前显示的cell
public func visibleCells() -> [UICollectionViewCell]
```

## 7 Inserting, Moving, and Deleting Items

```swift
// 插入Items
public func insertItemsAtIndexPaths(indexPaths: [NSIndexPath])
// 移除Items
public func deleteItemsAtIndexPaths(indexPaths: [NSIndexPath])
// 移动Items
public func moveItemAtIndexPath(indexPath: NSIndexPath, toIndexPath newIndexPath: NSIndexPath)    
```

## 8 Inserting, Moving, and Deleting Sections

```swift
// 插入sections
public func insertSections(sections: NSIndexSet)
// 移除sections
public func deleteSections(sections: NSIndexSet)
// 移动sections
public func moveSection(section: Int, toSection newSection: Int)
```

## 9 Reordering Items Interactively

```swift
// 能否交互移动item
@available(iOS 9.0, *)
public func beginInteractiveMovementForItemAtIndexPath(indexPath: NSIndexPath) -> Bool
// item移动到某个点
@available(iOS 9.0, *)
public func updateInteractiveMovementTargetPosition(targetPosition: CGPoint)
// 结束移动
@available(iOS 9.0, *)
public func endInteractiveMovement()
// 取消交互移动
@available(iOS 9.0, *)
public func cancelInteractiveMovement()
```

## 10 Managing the Selection

```swift
// 能否选中
public var allowsSelection: Bool
// 能否选中多个item
public var allowsMultipleSelection: Bool    
// 当前选中的item
public func indexPathsForSelectedItems() -> [NSIndexPath]?
// 选中item
public func selectItemAtIndexPath(indexPath: NSIndexPath?, animated: Bool, scrollPosition: UICollectionViewScrollPosition)
// 取消选中item
public func deselectItemAtIndexPath(indexPath: NSIndexPath, animated: Bool)
```

## 11 Managing Focus

```swift
// 检查焦点是否应该返回上一个焦点索引
@available(iOS 9.0, *)
public var remembersLastFocusedIndexPath: Bool
```

## 12 Locating Items and Views in the Collection View

```swift
// 根据点获取对应显示的item
public func indexPathForItemAtPoint(point: CGPoint) -> NSIndexPath?
// 当前显示的items
public func indexPathsForVisibleItems() -> [NSIndexPath]
// cell对应的位置
public func indexPathForCell(cell: UICollectionViewCell) -> NSIndexPath?
// 根据位置获取对应cell
public func cellForItemAtIndexPath(indexPath: NSIndexPath) -> UICollectionViewCell?
// 根据指定的标记获取对应的位置
@available(iOS 9.0, *)
public func indexPathsForVisibleSupplementaryElementsOfKind(elementKind: String) -> [NSIndexPath]
// 获取对应的组头或组尾
@available(iOS 9.0, *)
public func supplementaryViewForElementKind(elementKind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionReusableView
// 获取当前显示的组头或组尾
@available(iOS 9.0, *)
public func visibleSupplementaryViewsOfKind(elementKind: String) -> [UICollectionReusableView]
```

## 13 Getting Layout Information

```swift
// 获取item对应的约束
public func layoutAttributesForItemAtIndexPath(indexPath: NSIndexPath) -> UICollectionViewLayoutAttributes?
// 获取组头或组尾对应的约束
public func layoutAttributesForSupplementaryElementOfKind(kind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionViewLayoutAttributes?
```

## 14 Scrolling an Item Into View

```swift
// 滚动到指定的item
public func scrollToItemAtIndexPath(indexPath: NSIndexPath, atScrollPosition scrollPosition: UICollectionViewScrollPosition, animated: Bool)
```

## 15 Animating Multiple Changes to the Collection View

```swift
// 批处理执行insert/delete/reload/move
public func performBatchUpdates(updates: (() -> Void)?, completion: ((Bool) -> Void)?)
```

&#160;

----------

# Appendix

## Related Documentation

[UICollectionView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-07-25 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974