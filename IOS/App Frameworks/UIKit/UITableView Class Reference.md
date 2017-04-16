UITableView有两种样式UITableViewStylePlain和UITableViewStyleGrouped。

## 1 Initializing a UITableView Object

```swift
// 初始化
public init(frame: CGRect, style: UITableViewStyle)
```

## 2 Configuring a Table View

```swift
// 展示样式
public var style: UITableViewStyle { get }
// 显示几个组
public var numberOfSections: Int { get }
// 每组内显示的个数
public func numberOfRowsInSection(section: Int) -> Int
// 每行显示高度
public var rowHeight: CGFloat
// 分割线样式
public var separatorStyle: UITableViewCellSeparatorStyle
// 分割线颜色
public var separatorColor: UIColor?
// 毛玻璃效果
@available(iOS 8.0, *)
@NSCopying public var separatorEffect: UIVisualEffect?
// 背景View
@available(iOS 3.2, *)
public var backgroundView: UIView?
// 分离的大小
@available(iOS 7.0, *)
public var separatorInset: UIEdgeInsets
// iPad左侧宽度设置
@available(iOS 9.0, *)
public var cellLayoutMarginsFollowReadableWidth: Bool 
```

## 3 Creating Table View Cells

```swift
// 注册xib创建的cell
@available(iOS 5.0, *)
public func registerNib(nib: UINib?, forCellReuseIdentifier identifier: String)
// 注册class创建的cell
@available(iOS 6.0, *)
public func registerClass(cellClass: AnyClass?, forCellReuseIdentifier identifier: String)
// 根据标识符从重用队列获取cell
public func dequeueReusableCellWithIdentifier(identifier: String) -> UITableViewCell?
// 根据标识符和位置从重用队列获取cell
@available(iOS 6.0, *)
public func dequeueReusableCellWithIdentifier(identifier: String, forIndexPath indexPath: NSIndexPath) -> UITableViewCell
```

## 4 Accessing Header and Footer Views

```swift
// 注册通过xib创建Section头部和尾部
@available(iOS 6.0, *)
public func registerNib(nib: UINib?, forHeaderFooterViewReuseIdentifier identifier: String)
// 注册通过class创建Section头部和尾部
@available(iOS 6.0, *)
public func registerClass(aClass: AnyClass?, forHeaderFooterViewReuseIdentifier identifier: String)
// 通过标识符从重用队列获取Section头部或尾部
@available(iOS 6.0, *)
public func dequeueReusableHeaderFooterViewWithIdentifier(identifier: String) -> UITableViewHeaderFooterView?
// table头部
public var tableHeaderView: UIView?
// table尾部
public var tableFooterView: UIView?
// Section头部高度
public var sectionHeaderHeight: CGFloat
// Section尾部高度
public var sectionFooterHeight: CGFloat 
// 通过组获取Section头部
@available(iOS 6.0, *)
public func headerViewForSection(section: Int) -> UITableViewHeaderFooterView?
// 通过组获取Section尾部
@available(iOS 6.0, *)
public func footerViewForSection(section: Int) -> UITableViewHeaderFooterView?
```

## 5 Accessing Cells and Sections

```swift
// 根据位置获取UITableViewCell
public func cellForRowAtIndexPath(indexPath: NSIndexPath) -> UITableViewCell?
// 根据cell获取其所在位置
public func indexPathForCell(cell: UITableViewCell) -> NSIndexPath? 
// 根据点获取cell对应的位置
public func indexPathForRowAtPoint(point: CGPoint) -> NSIndexPath?
// 获取显示区域的cell
public func indexPathsForRowsInRect(rect: CGRect) -> [NSIndexPath]?
// 当前显示的cell
public var visibleCells: [UITableViewCell] { get }
// 获取显示cell对应的位置
public var indexPathsForVisibleRows: [NSIndexPath]? { get }
```

## 6 Estimating Element Heights

```swift
// 估计的row显示高度
@available(iOS 7.0, *)
public var estimatedRowHeight: CGFloat
// 估计的SectionHeaderHeight
@available(iOS 7.0, *)
public var estimatedSectionHeaderHeight: CGFloat
// 估计的SectionFooterHeight
@available(iOS 7.0, *)
public var estimatedSectionFooterHeight: CGFloat
```

## 7 Scrolling the Table View

```swift
// 滚动到指定位置
public func scrollToRowAtIndexPath(indexPath: NSIndexPath, atScrollPosition scrollPosition: UITableViewScrollPosition, animated: Bool)
// 滚动到选中的cell
public func scrollToNearestSelectedRowAtScrollPosition(scrollPosition: UITableViewScrollPosition, animated: Bool)
```

## 8 Managing Selections

```swift
// 获取选中的cell
public var indexPathForSelectedRow: NSIndexPath? { get }
// 获取选中的多个cell
@available(iOS 5.0, *)
public var indexPathsForSelectedRows: [NSIndexPath]? { get }
// 选中cell
public func selectRowAtIndexPath(indexPath: NSIndexPath?, animated: Bool, scrollPosition: UITableViewScrollPosition)
// 取消选中Cell
public func deselectRowAtIndexPath(indexPath: NSIndexPath, animated: Bool)
// 是否允许选中cell
@available(iOS 3.0, *)
public var allowsSelection: Bool
// 编辑状态能否选中cell
public var allowsSelectionDuringEditing: Bool
// 能否选中多个Cell
@available(iOS 5.0, *)
public var allowsMultipleSelection: Bool
// 编辑状态能否选中多个cell
@available(iOS 5.0, *)
public var allowsMultipleSelectionDuringEditing: Bool
```

## 9 Inserting, Deleting, and Moving Rows and Sections

```swift
// 开始更新
public func beginUpdates()
// 结束更新
public func endUpdates()
// 插入组
public func insertSections(sections: NSIndexSet, withRowAnimation animation: UITableViewRowAnimation)
// 移除组
public func deleteSections(sections: NSIndexSet, withRowAnimation animation: UITableViewRowAnimation)
// 移动组
@available(iOS 5.0, *)
public func moveSection(section: Int, toSection newSection: Int)
// 插入cells
public func insertRowsAtIndexPaths(indexPaths: [NSIndexPath], withRowAnimation animation: UITableViewRowAnimation)
// 移除cells
public func deleteRowsAtIndexPaths(indexPaths: [NSIndexPath], withRowAnimation animation: UITableViewRowAnimation)
@available(iOS 3.0, *)
// 移动cells
public func moveRowAtIndexPath(indexPath: NSIndexPath, toIndexPath newIndexPath: NSIndexPath)
```

## 10  Managing the Editing of Table Cells

```swift
// 设置编辑状态
public var editing: Bool
// 动画设置编辑状态
public func setEditing(editing: Bool, animated: Bool)
```

## 11  Reloading the Table View

```swift
// 更新数据源
public func reloadData()
// 更新指引条
@available(iOS 3.0, *)
public func reloadSectionIndexTitles()
// 更新组
@available(iOS 3.0, *)
public func reloadSections(sections: NSIndexSet, withRowAnimation animation: UITableViewRowAnimation)
// 更新cells
public func reloadRowsAtIndexPaths(indexPaths: [NSIndexPath], withRowAnimation animation: UITableViewRowAnimation)
```

## 12  Accessing Drawing Areas of the Table View

```swift
// 组显示区域
public func rectForSection(section: Int) -> CGRect
// 组头部显示区域
public func rectForHeaderInSection(section: Int) -> CGRect
// 组尾部显示区域
public func rectForFooterInSection(section: Int) -> CGRect
// cell显示区域
public func rectForRowAtIndexPath(indexPath: NSIndexPath) -> CGRect
```

## 13  Managing the Delegate and the Data Source

```swift
// 数据源代理
weak public var dataSource: UITableViewDataSource?
// 配置操作代理
weak public var delegate: UITableViewDelegate?
```

## 14  Configuring the Table Index

```swift
// 最少显示的cell数目
public var sectionIndexMinimumDisplayRowCount: Int
// 索引的颜色
@available(iOS 6.0, *)
public var sectionIndexColor: UIColor?
// 索引的背景色
@available(iOS 7.0, *)
public var sectionIndexBackgroundColor: UIColor?
// 跟踪的索引显示颜色
@available(iOS 6.0, *)
public var sectionIndexTrackingBackgroundColor: UIColor?
```

## 15  Managing Focus

```swift
// 检查焦点是否应该返回上一个焦点索引
@available(iOS 9.0, *)
public var remembersLastFocusedIndexPath: Bool
```

&#160;

----------

# Appendix

## Related Documentation

[UITableView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/index.html)


## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-07-25 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974