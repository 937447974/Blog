**1 [简介](#简介)**


**2 [项目准备](#项目准备)**

**3 [UITableViewDelegate实现](#UITableViewDelegate实现)**

1. [配置表试图的行](#配置表试图的行)
2. [管理附属视图](#管理附属视图)
3. [管理选择](#管理选择)
4. [修改组Header和Footer](#修改组Header和Footer)
5. [编辑行](#编辑行)
6. [重排行](#重排行)
7. [追踪移除视图](#追踪移除视图)
8. [复制和粘贴行内容](#复制和粘贴行内容)
9. [管理表视图高亮](#管理表视图高亮)
10. [管理表视图焦点](#管理表视图焦点)

---

#1<a id="简介"/> 简介

当我们使用UITableView对象时，可以实现UITableViewDelegate协议。该协议可以管理组、配置组标题和脚标、帮助我们删除和重排cell对象，还有一些其他操作。

使用UITableViewDelegate的时候多数情况会使用NSIndexPath对象作为当前行。其中有两个核心参数section(组)和row(行)。

#2<a id="项目准备"/> 项目准备

使用YJTableViewDelegateVC.swift做基础测试，这里已经优先实现了UITableViewDataSource部分协议，至于界面可自行搭建。

```swift
//
//  YJTableViewDelegateVC.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/14.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// 打印测试
private enum YJPrintStyle: Int {
    case ConfiguringRowsForTheTableView
    case ManagingAccessoryViews
    case ManagingSelections
    case ModifyingTheHeaderAndFooterOfSections
    case EditingTableRows
    case ReorderingTableRows
    case TrackingTheRemovalOfViews
    case CopyingAndPastingRowContent
    case ManagingTableViewHighlighting
    case ManagingTableViewFocus
}

/// UITableViewDelegate演示
class YJTableViewDelegateVC: UIViewController, UITableViewDataSource, UITableViewDelegate {
    
    /// 数据源
    var data = [[Int]]()
    /// UITableView
    @IBOutlet weak var tableView: UITableView!
    /// 打印测试
    private var printStyle: YJPrintStyle!
    
    // MARK: - view
    override func viewDidLoad() {
        super.viewDidLoad()
        self.printStyle = .ManagingTableViewFocus
        // 以组演示,填充相关测试数据
        var section = [Int]()
        for _ in 0..<5 {
            section.removeAll()
            for row in 0..<10 {
                section.append(row)
            }
            self.data.append(section)
        }
        // self.tableView.editing = true
    }
    
    // MARK: - UITableViewDataSource
    // MARK: 有几组
    func numberOfSectionsInTableView(tableView: UITableView) -> Int  {
        return self.data.count
    }
    
    // MARK: 每一组有几个元素
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.data[section].count
    }
    
    // MARK: 生成Cell
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        var cell = tableView.dequeueReusableCellWithIdentifier("cell")
        if cell == nil {
            cell = UITableViewCell(style: UITableViewCellStyle.Default, reuseIdentifier: "cell")
            cell?.accessoryType = UITableViewCellAccessoryType.DetailButton
        }
        cell?.textLabel?.text = "\(self.data[indexPath.section][indexPath.row])--\(indexPath.section)"
        print(indexPath)
        return cell!
    }
    
    // MARK: 索引
    func sectionIndexTitlesForTableView(tableView: UITableView) -> [String]? {
        var sectionTitles = [String]()
        for i in 0..<self.data.count {
            sectionTitles.append("\(i)")
        }
        return sectionTitles
    }
    
    // MARK: 索引对应的组
    func tableView(tableView: UITableView, sectionForSectionIndexTitle title: String, atIndex index: Int) -> Int {
        return Int(title) ?? 0
    }
    
}
```

#3<a id="UITableViewDelegate实现"/> UITableViewDelegate实现

##3.1<a id="配置表试图的行"/> 配置表试图的行

```swift
// MARK: 预获取行高
func tableView(tableView: UITableView, estimatedHeightForRowAtIndexPath indexPath: NSIndexPath) -> CGFloat {
    // 实现此方法后,显示界面前func tableView(tableView: UITableView, heightForRowAtIndexPath indexPath: NSIndexPath) -> CGFloath不会执行，有助于提升显示效率
    if self.printStyle == .ConfiguringRowsForTheTableView {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    return tableView.estimatedRowHeight
}
    
// MARK: 行缩进
func tableView(tableView: UITableView, indentationLevelForRowAtIndexPath indexPath: NSIndexPath) -> Int {
    if self.printStyle == .ConfiguringRowsForTheTableView {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    return self.data[indexPath.section][indexPath.row]
}
    
// MARK: 行高
func tableView(tableView: UITableView, heightForRowAtIndexPath indexPath: NSIndexPath) -> CGFloat {
    if self.printStyle == .ConfiguringRowsForTheTableView {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    // 默认44
    return tableView.rowHeight
}
    
// MARK: 将要显示行
func tableView(tableView: UITableView, willDisplayCell cell: UITableViewCell, forRowAtIndexPath indexPath: NSIndexPath) {
    if self.printStyle == .ConfiguringRowsForTheTableView {
        print("\(__FUNCTION__)--\(indexPath)")
    }
}
```

##3.2<a id="管理附属视图"/> 管理附属视图

```swift
// MARK: 左滑出现的按钮
func tableView(tableView: UITableView, editActionsForRowAtIndexPath indexPath: NSIndexPath) -> [UITableViewRowAction]? {
    if self.printStyle == .ManagingAccessoryViews {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    var action = [UITableViewRowAction]()
    let handler: (UITableViewRowAction, NSIndexPath) -> Void = {(action: UITableViewRowAction, indexPath: NSIndexPath) in
        print("\(indexPath)--\(action.title)")
    }
    action.append(UITableViewRowAction(style: UITableViewRowActionStyle.Default, title: "Default", handler:handler))
    action.append(UITableViewRowAction(style: UITableViewRowActionStyle.Destructive, title: "Destructive", handler:handler))
    action.append(UITableViewRowAction(style: UITableViewRowActionStyle.Normal, title: "Normal", handler:handler))
    return action
}
    
// MARK: 点击cell上的系统按钮
func tableView(tableView: UITableView, accessoryButtonTappedForRowWithIndexPath indexPath: NSIndexPath) {
    // cell?.accessoryType = UITableViewCellAccessoryType.DetailButton
    if self.printStyle == .ManagingAccessoryViews {
        print("\(__FUNCTION__)--\(indexPath)")
    }
}
```

##3.3<a id="管理选择"/> 管理选择

```swift
// MARK: 将要点击某行
func tableView(tableView: UITableView, willSelectRowAtIndexPath indexPath: NSIndexPath) -> NSIndexPath? {
    if self.printStyle == .ManagingSelections {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    // 返回nil代表不能点击
    return indexPath
}
    
// MARK: 点击某行
func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
    if self.printStyle == .ManagingSelections {
        print("\(__FUNCTION__)--\(indexPath)")
    }
}
    
// MARK: 将要删除某行
func tableView(tableView: UITableView, willDeselectRowAtIndexPath indexPath: NSIndexPath) -> NSIndexPath? {
    if self.printStyle == .ManagingSelections {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    // 返回nil代表不能删除
    return indexPath
}
    
// MARK: 删除某行
func tableView(tableView: UITableView, didDeselectRowAtIndexPath indexPath: NSIndexPath) {
    if self.printStyle == .ManagingSelections {
        print("\(__FUNCTION__)--\(indexPath)")
    }
}
```

##3.4<a id="修改组Header和Footer"/> 修改组Header和Footer

```swift
// MARK: 预获取header高
func tableView(tableView: UITableView, estimatedHeightForHeaderInSection section: Int) -> CGFloat {
    if self.printStyle == .ModifyingTheHeaderAndFooterOfSections {
        print("\(__FUNCTION__)--\(section)")
    }
    return tableView.estimatedSectionHeaderHeight
}
    
// MARK: 预获取footer高
func tableView(tableView: UITableView, estimatedHeightForFooterInSection section: Int) -> CGFloat {
    if self.printStyle == .ModifyingTheHeaderAndFooterOfSections {
        print("\(__FUNCTION__)--\(section)")
    }
    return tableView.estimatedSectionFooterHeight
}
    
// MARK: header高
func tableView(tableView: UITableView, heightForHeaderInSection section: Int) -> CGFloat {
    if self.printStyle == .ModifyingTheHeaderAndFooterOfSections {
        print("\(__FUNCTION__)--\(section)")
    }
    return tableView.sectionHeaderHeight
}
    
// MARK: footer高
func tableView(tableView: UITableView, heightForFooterInSection section: Int) -> CGFloat {
    if self.printStyle == .ModifyingTheHeaderAndFooterOfSections {
        print("\(__FUNCTION__)--\(section)")
    }
    return tableView.sectionFooterHeight
}
    
// MARK: 自定义header
func tableView(tableView: UITableView, viewForHeaderInSection section: Int) -> UIView? {
    if self.printStyle == .ModifyingTheHeaderAndFooterOfSections {
        print("\(__FUNCTION__)--\(section)")
    }
    let view = UIView(frame: CGRectMake(0, 0, YJUtilScreenSize.screenWidth, tableView.sectionHeaderHeight))
    view.backgroundColor = UIColor.redColor()
    return view
}
    
// MARK: 自定义footer
func tableView(tableView: UITableView, viewForFooterInSection section: Int) -> UIView? {
    if self.printStyle == .ModifyingTheHeaderAndFooterOfSections {
        print("\(__FUNCTION__)--\(section)")
    }
    let view = UIView(frame: CGRectMake(0, 0, YJUtilScreenSize.screenWidth, tableView.sectionFooterHeight))
    view.backgroundColor = UIColor.yellowColor()
    return view
}
    
// MARK: 将要显示header
func tableView(tableView: UITableView, willDisplayHeaderView view: UIView, forSection section: Int) {
    if self.printStyle == .ModifyingTheHeaderAndFooterOfSections {
        print("\(__FUNCTION__)--\(section)")
    }
}
    
// MARK: 将要显示footer
func tableView(tableView: UITableView, willDisplayFooterView view: UIView, forSection section: Int) {
    if self.printStyle == .ModifyingTheHeaderAndFooterOfSections {
        print("\(__FUNCTION__)--\(section)")
    }
}
```

##3.5<a id="编辑行"/> 编辑行

```swift
// MARK: 将要进入编辑模式
func tableView(tableView: UITableView, willBeginEditingRowAtIndexPath indexPath: NSIndexPath) {
    if self.printStyle == .EditingTableRows {
        print("\(__FUNCTION__)--\(indexPath)")
    }
}
    
// MARK: 退出编辑模式
func tableView(tableView: UITableView, didEndEditingRowAtIndexPath indexPath: NSIndexPath) {
    if self.printStyle == .EditingTableRows {
        print("\(__FUNCTION__)--\(indexPath)")
    }
}
    
// MARK: 编辑模式执行的操作
func tableView(tableView: UITableView, editingStyleForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCellEditingStyle {
    if self.printStyle == .EditingTableRows {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    return UITableViewCellEditingStyle.Delete
}
    
// MARK: 更改默认的删除按钮标题
func tableView(tableView: UITableView, titleForDeleteConfirmationButtonForRowAtIndexPath indexPath: NSIndexPath) -> String? {
    if self.printStyle == .EditingTableRows {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    return "自定义"
}
    
// MARK: 编辑模式下表视图背景是否缩进
func tableView(tableView: UITableView, shouldIndentWhileEditingRowAtIndexPath indexPath: NSIndexPath) -> Bool {
    if self.printStyle == .EditingTableRows {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    return true
}
```

##3.6<a id="重排行"/> 重排行

```swift
// MARK: 移动cell
func tableView(tableView: UITableView, targetIndexPathForMoveFromRowAtIndexPath sourceIndexPath: NSIndexPath, toProposedIndexPath proposedDestinationIndexPath: NSIndexPath) -> NSIndexPath {
    if self.printStyle == .ReorderingTableRows {
        print("\(__FUNCTION__)--\(sourceIndexPath)--\(proposedDestinationIndexPath)")
    }
    return proposedDestinationIndexPath
}
```

##3.7<a id="追踪移除视图"/> 追踪移除视图

```swift
// MARK: header消失
func tableView(tableView: UITableView, didEndDisplayingHeaderView view: UIView, forSection section: Int) {
    if self.printStyle == .TrackingTheRemovalOfViews {
        print("\(__FUNCTION__)--\(section)")
    }
}
    
// MARK: cell消失
func tableView(tableView: UITableView, didEndDisplayingCell cell: UITableViewCell, forRowAtIndexPath indexPath: NSIndexPath) {
    if self.printStyle == .TrackingTheRemovalOfViews {
        print("\(__FUNCTION__)--\(indexPath)")
    }
}
    
// MARK: footer消失
func tableView(tableView: UITableView, didEndDisplayingFooterView view: UIView, forSection section: Int) {
    if self.printStyle == .TrackingTheRemovalOfViews {
        print("\(__FUNCTION__)--\(section)")
    }
}
```

##3.8<a id="复制和粘贴行内容"/> 复制和粘贴行内容

```swift
// 将要显示复制和粘贴板
func tableView(tableView: UITableView, shouldShowMenuForRowAtIndexPath indexPath: NSIndexPath) -> Bool {
    if self.printStyle == .CopyingAndPastingRowContent {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    return true
}
    
// 显示复制和粘贴板
func tableView(tableView: UITableView, canPerformAction action: Selector, forRowAtIndexPath indexPath: NSIndexPath, withSender sender: AnyObject?) -> Bool {
    if self.printStyle == .CopyingAndPastingRowContent {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    return true
}
    
// MARK: 点击复制和粘贴板上的按钮
func tableView(tableView: UITableView, performAction action: Selector, forRowAtIndexPath indexPath: NSIndexPath, withSender sender: AnyObject?) {
    if self.printStyle == .CopyingAndPastingRowContent {
        print("\(__FUNCTION__)--\(indexPath)--\(action)--\(sender)")
    }
}
```

##3.9<a id="管理表视图高亮"/> 管理表视图高亮

```swift
// MARK: 点击cell能否进入高亮模式
func tableView(tableView: UITableView, shouldHighlightRowAtIndexPath indexPath: NSIndexPath) -> Bool {
    if self.printStyle == .ManagingTableViewHighlighting {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    return true
}
    
// MARK: 正式进入高亮模式
func tableView(tableView: UITableView, didHighlightRowAtIndexPath indexPath: NSIndexPath) {
    if self.printStyle == .ManagingTableViewHighlighting {
        print("\(__FUNCTION__)--\(indexPath)")
    }
}
    
// MARK: 离开高亮模式
func tableView(tableView: UITableView, didUnhighlightRowAtIndexPath indexPath: NSIndexPath) {
    if self.printStyle == .ManagingTableViewHighlighting {
        print("\(__FUNCTION__)--\(indexPath)")
    }
}
```

##3.10<a id="管理表视图焦点"/> 管理表视图焦点

```swift
// MARK: 是否指定路径
func tableView(tableView: UITableView, canFocusRowAtIndexPath indexPath: NSIndexPath) -> Bool {
    if self.printStyle == .ManagingTableViewFocus {
        print("\(__FUNCTION__)--\(indexPath)")
    }
    return true
}
    
// MARK: 是否允许上下文更新
func tableView(tableView: UITableView, shouldUpdateFocusInContext context: UITableViewFocusUpdateContext) -> Bool {
    if self.printStyle == .ManagingTableViewFocus {
        print("\(__FUNCTION__)--\(context)")
    }
    return true
}
    
// MARK: 上下文更新结束
func tableView(tableView: UITableView, didUpdateFocusInContext context: UITableViewFocusUpdateContext, withAnimationCoordinator coordinator: UIFocusAnimationCoordinator) {
    if self.printStyle == .ManagingTableViewFocus {
        print("\(__FUNCTION__)--\(context)")
    }
}
    
// MARK: 索引对应的路径
func indexPathForPreferredFocusedViewInTableView(tableView: UITableView) -> NSIndexPath? {
    if self.printStyle == .ManagingTableViewFocus {
        print("\(__FUNCTION__)")
    }
    return nil
}
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UITableViewDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-15 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog