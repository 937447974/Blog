[UICollectionViewDataSource](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDataSource.md)

[UICollectionViewDelegate](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegate.md)

[UICollectionViewDelegateFlowLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegateFlowLayout.md)

[UICollectionViewCell](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewCell.md)

[UICollectionViewLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewLayout.md)

---

1. [管理选择Cells](#管理选择Cells)
2. [管理Cell高亮](#管理Cell高亮)
3. [增加和移除视图](#增加和移除视图)
4. [管理布局变化](#管理布局变化)
5. [管理Cells长点击事件](#管理Cells长点击事件)

---

UICollectionViewDelegate能够帮助我们更加精细的管理UICollectionView，如管理选择Cells、

>所有的协议方法都是optional。

#<a id="管理选择Cells"/>1 管理选择Cells

```swift
// MARK: 是否选中某个item
func collectionView(_ collectionView: UICollectionView, shouldSelectItemAtIndexPath indexPath: NSIndexPath) -> Bool

// MARK: 选中某个item
func collectionView(_ collectionView: UICollectionView, didSelectItemAtIndexPath indexPath: NSIndexPath)

// MARK: 是否取消选中某个item
func collectionView(collectionView: UICollectionView, shouldDeselectItemAtIndexPath indexPath: NSIndexPath) -> Bool 
    
// MARK: 取消选中
func collectionView(collectionView: UICollectionView, didDeselectItemAtIndexPath indexPath: NSIndexPath)
```

#<a id="管理Cell高亮"/>2 管理Cell高亮

```swift
// MARK: 能否选中高亮
func collectionView(collectionView: UICollectionView, shouldHighlightItemAtIndexPath indexPath: NSIndexPath) -> Bool
    
// MARK: 高亮
func collectionView(collectionView: UICollectionView, didHighlightItemAtIndexPath indexPath: NSIndexPath)

// MARK: 高亮取消
func collectionView(collectionView: UICollectionView, didUnhighlightItemAtIndexPath indexPath: NSIndexPath)
```

#<a id="增加和移除视图"/>3 增加和移除视图

```swift
// MARK: cell显示
func collectionView(collectionView: UICollectionView, willDisplayCell cell: UICollectionViewCell, forItemAtIndexPath indexPath: NSIndexPath)

// MARK: cell消失
func collectionView(collectionView: UICollectionView, didEndDisplayingCell cell: UICollectionViewCell, forItemAtIndexPath indexPath: NSIndexPath)
   
// MARK: Header或Footer显示
func collectionView(collectionView: UICollectionView, willDisplaySupplementaryView view: UICollectionReusableView, forElementKind elementKind: String, atIndexPath indexPath: NSIndexPath)
    
// MARK: Header或Footer消失
func collectionView(collectionView: UICollectionView, didEndDisplayingSupplementaryView view: UICollectionReusableView, forElementOfKind elementKind: String, atIndexPath indexPath: NSIndexPath)
```

#<a id="管理布局变化"/>4 管理布局变化

```swift
// MARK: collectionViewLayout: UICollectionViewLayout发生改变
func collectionView(collectionView: UICollectionView, transitionLayoutForOldLayout fromLayout: UICollectionViewLayout, newLayout toLayout: UICollectionViewLayout) -> UICollectionViewTransitionLayout
    
// MARK: cell可移动时的起始偏移
func collectionView(collectionView: UICollectionView, targetContentOffsetForProposedContentOffset proposedContentOffset: CGPoint) -> CGPoint {
    if self.yjPrint == YJPrint.HandlingLayoutChanges 
    
// MARK: 移动cell从originalIndexPath到proposedIndexPath
func collectionView(collectionView: UICollectionView, targetIndexPathForMoveFromItemAtIndexPath originalIndexPath: NSIndexPath, toProposedIndexPath proposedIndexPath: NSIndexPath) -> NSIndexPath
```

#<a id="管理Cells长点击事件"/>5 管理Cells长点击事件

```swift
// MARK: 长按，是否将要显示Action菜单（剪切、拷贝、粘贴）
func collectionView(collectionView: UICollectionView, shouldShowMenuForItemAtIndexPath indexPath: NSIndexPath) -> Bool
    
// MARK: 长按，是否显示Action菜单（剪切、拷贝、粘贴）
func collectionView(collectionView: UICollectionView, canPerformAction action: Selector, forItemAtIndexPath indexPath: NSIndexPath, withSender sender: AnyObject?) -> Bool 
    
// MARK: 点击菜单中的某个选项
func collectionView(collectionView: UICollectionView, performAction action: Selector, forItemAtIndexPath indexPath: NSIndexPath, withSender sender: AnyObject?) {
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UICollectionView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/index.html)

[UICollectionViewDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewDelegate_protocol/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-23 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog