[UICollectionViewDataSource](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDataSource.md)

[UICollectionViewDelegate](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegate.md)

[UICollectionViewDelegateFlowLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegateFlowLayout.md)

[UICollectionViewCell](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewCell.md)

[UICollectionViewLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewLayout.md)

---

1. [UICollectionViewLayout](#UICollectionViewLayout)
2. [自定义UICollectionViewLayout](#自定义UICollectionViewLayout)
3. [实战演练](#实战演练)

----

#<a id="UICollectionViewLayout"/>1 UICollectionViewLayout

UICollectionViewLayout就是UICollectionView显示用的布局文件。我们使用过程中更多的是使用它的子类UICollectionViewFlowLayout和UICollectionViewTransitionLayout。

1. UICollectionViewFlowLayout：显示布局。
2. UICollectionViewTransitionLayout：过渡布局。

开发过程中用的最多的是UICollectionViewFlowLayout，这里不在详细描述。今天主要为大家讲解自定义UICollectionViewLayout，是模仿UICollectionViewFlowLayout制作的。通过自定义你可以显示各种各样的布局。

要实现UICollectionViewLayout自定义需要实现下列方法。

```swift
// MARK: - Getting the Collection View Information
// MARK: 返回集合视图的高度和宽度
override public func collectionViewContentSize() -> CGSize
    
// MARK: - Invalidating the Layout
// MARK: 是否需要布局更新
override public func shouldInvalidateLayoutForBoundsChange (newBounds : CGRect) -> Bool
    
// MARK: - Providing Layout Attributes
// MARK: 返回指定矩形中所有单元格和视图的布局属性
override public func layoutAttributesForElementsInRect(rect: CGRect) -> [UICollectionViewLayoutAttributes]?
    
// MARK: 返回指定索引路径中项目的布局属性
override public func layoutAttributesForItemAtIndexPath(indexPath: NSIndexPath) -> UICollectionViewLayoutAttributes?
    
// MARK: 返回指定的装饰视图的布局属性
public override func layoutAttributesForDecorationViewOfKind(elementKind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionViewLayoutAttributes?
    
// MARK: 返回指定的附加视图的布局属性
override public func layoutAttributesForSupplementaryViewOfKind(elementKind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionViewLayoutAttributes?
    
// MARK: - 更新布局
override public func prepareLayout()
```

当然还有其他的方法，可自行实现。这些是主要的方法。

#<a" id="自定义UICollectionViewLayout"/>2 自定义UICollectionViewLayout

下面为大家展示所有源代码。

```swift
//
//  YJCollectionViewFlowLayout.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/22.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// 自定义UICollectionViewDelegateFlowLayout
@objc public protocol YJCollectionViewDelegateFlowLayout: UICollectionViewDelegate, UICollectionViewDataSource {
    
    /// 获取cell高度
    ///
    /// - parameter collectionView: UICollectionView
    /// - parameter collectionViewFlowLayout: YJCollectionViewFlowLayout
    /// - parameter indexPath: NSIndexPath
    ///
    /// - returns: CGFloat
    func collectionView(collectionView: UICollectionView, layout collectionViewFlowLayout: YJCollectionViewFlowLayout, heightForItemAtIndexPath indexPath: NSIndexPath) -> CGFloat
    
    /// 获取列宽
    ///
    /// - parameter collectionView: UICollectionView
    /// - parameter collectionViewFlowLayout: YJCollectionViewFlowLayout
    /// - parameter column: 列
    ///
    /// - returns: CGFloat
    optional func collectionView(collectionView: UICollectionView, layout collectionViewFlowLayout: YJCollectionViewFlowLayout, widthForSectionAtColumn column: Int) -> CGFloat
    
}

/// 自定义UICollectionViewFlowLayout
public class YJCollectionViewFlowLayout : UICollectionViewLayout{
    
    // MARK: - 默认internal属性
    /// header高度
    var headerReferenceHeight: CGFloat = 0.0
    /// footer高度
    var footerReferenceHeight: CGFloat = 0.0
    /// section距边
    var sectionInset: UIEdgeInsets = UIEdgeInsets()
    /// 每列item的宽
    var columnItemWidths = [CGFloat]()
    /// section中的小块
    var sectionItems = [[Int]]() {
        didSet {
            var count = 0
            for list in sectionItems {
                count += list.count
            }
            self.sectionItemsCount = count
            self.invalidateLayout() // 更新布局
        }
    }
    // MARK: - private属性
    /// section中的小块个数,{get}
    var sectionItemsCount = 0
    /// 行间隔
    private var lineSpacing: CGFloat = 0.0
    /// 列间隔
    private var interitemSpacing: CGFloat = 0.0
    /// YJCollectionViewDelegateFlowLayout协议
    private weak var delegateFlowLayout : YJCollectionViewDelegateFlowLayout?{
        get{
            return self.collectionView?.delegate as? YJCollectionViewDelegateFlowLayout
        }
    }
    /// 显示所需高度
    private var flowHeight: CGFloat = 0.0
    /// 所有的UICollectionViewLayoutAttributes
    private var allItemAttributes = [UICollectionViewLayoutAttributes]()
    /// Header的UICollectionViewLayoutAttributes
    private var headersAttributes = [UICollectionViewLayoutAttributes]()
    /// SectionItem的UICollectionViewLayoutAttributes
    private var sectionItemAttributes = [[UICollectionViewLayoutAttributes]]()
    /// Footer的UICollectionViewLayoutAttributes
    private var footersAttributes = [UICollectionViewLayoutAttributes]()
    
    // MARK: - Getting the Collection View Information
    // MARK: 返回集合视图的高度和宽度
    override public func collectionViewContentSize() -> CGSize{
        var contentSize = self.collectionView!.bounds.size
        contentSize.height = self.flowHeight
        return contentSize
    }
    
    // MARK: - Invalidating the Layout
    // MARK: 是否需要布局更新
    override public func shouldInvalidateLayoutForBoundsChange (newBounds : CGRect) -> Bool {
        let oldBounds = self.collectionView!.bounds
        if CGRectGetWidth(newBounds) != CGRectGetWidth(oldBounds){
            return true
        }
        return false
    }
    
    // MARK: - Providing Layout Attributes
    // MARK: 返回指定矩形中所有单元格和视图的布局属性
    override public func layoutAttributesForElementsInRect(rect: CGRect) -> [UICollectionViewLayoutAttributes]? {
        var beginIndex = 0
        var endIndex = self.allItemAttributes.count
        var beginIntersects = false
        var endIntersects = false
        // 寻找显示区域坐标
        for var index in 0..<endIndex {
            // 首个index
            if !beginIntersects && CGRectIntersectsRect(rect, self.allItemAttributes[index].frame) {
                beginIntersects = true
                beginIndex = index;
            }
            // 尾部id
            index = endIndex - 1 - index
            if !endIntersects && CGRectIntersectsRect(rect, self.allItemAttributes[index].frame){
                endIntersects = true
                endIndex = index+1
                break
            }
            // 都找到提前结束for循环
            if beginIntersects && endIntersects {
                break
            }
        }
        // 数据组装
        var layoutAttributes = [UICollectionViewLayoutAttributes]()
        for index in beginIndex..<endIndex {
            let attr = self.allItemAttributes[index]
            if CGRectIntersectsRect(rect, attr.frame) {
                layoutAttributes.append(attr)
            }
        }
        return layoutAttributes
    }
    
    // MARK: 返回指定索引路径中项目的布局属性
    override public func layoutAttributesForItemAtIndexPath(indexPath: NSIndexPath) -> UICollectionViewLayoutAttributes? {
        guard self.sectionItemAttributes.count > indexPath.section else {
            print("error: NSIndexPath.section, Array index out of range")
            return nil
        }
        guard self.sectionItemAttributes[indexPath.section].count > indexPath.item else {
            print("error: NSIndexPath.item, Array index out of range")
            return nil
        }
        return self.sectionItemAttributes[indexPath.section][indexPath.row]
    }
    
    // MARK: 返回指定的装饰视图的布局属性
    public override func layoutAttributesForDecorationViewOfKind(elementKind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionViewLayoutAttributes? {
        switch elementKind {
        case UICollectionElementKindSectionHeader:
            return self.headersAttributes[indexPath.section]
        case UICollectionElementKindSectionFooter:
            return self.footersAttributes[indexPath.section]
        default:
            return nil
        }
    }
    
    // MARK: 返回指定的附加视图的布局属性
    override public func layoutAttributesForSupplementaryViewOfKind(elementKind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionViewLayoutAttributes? {
        switch elementKind {
        case UICollectionElementKindSectionHeader:
            return self.headersAttributes[indexPath.section]
        case UICollectionElementKindSectionFooter:
            return self.footersAttributes[indexPath.section]
        default:
            return nil
        }
    }
    
    // MARK: - 更新布局
    override public func prepareLayout(){
        super.prepareLayout()
        /*
        * 1 相关数据清空
        */
        self.allItemAttributes.removeAll()
        self.headersAttributes.removeAll()
        self.footersAttributes.removeAll()
        self.sectionItemAttributes.removeAll()
        self.flowHeight = 0.0
        /*
        * 2 数据校验及准备工作
        */
        guard self.prepareLayoutGuard() else {
            return
        }
        /*
        *  3 加载显示用的UICollectionViewLayoutAttributes
        */
        for section in 0..<self.collectionView!.numberOfSections() {
            // 3.1 Section header
            self.prepareLayoutSectionHeader(section)
            // 3.2 Section items
            self.prepareLayoutSectionCell(section)
            // 3.3 Section footer
            self.prepareLayoutSectionFooter(section)
        }
    }
    
    // MARK: 更新布局时的数据校验
    private func prepareLayoutGuard() -> Bool {
        // 1 collectionView界面view
        guard self.collectionView != nil else {
            print("error: collectionView不存在")
            return false
        }
        // 2 delegateFlowLayout代理实现
        guard self.delegateFlowLayout != nil else {
            print("error: YJCollectionViewDelegateFlowLayout未实现")
            return false
        }
        // 3 numberOfSections有数据
        guard self.collectionView!.numberOfSections() != 0 else {
            print("error: numberOfSections = 0")
            return false
        }
        /*
        * 4 列宽处理
        */
        if var columnWidth = self.delegateFlowLayout?.collectionView?(self.collectionView!, layout: self, widthForSectionAtColumn: 0) {
            // 判断是否需要处理列宽
            self.columnItemWidths.removeAll()
            self.columnItemWidths.append(columnWidth)
            for i in 1..<self.sectionItems.count {
                columnWidth = self.delegateFlowLayout!.collectionView!(self.collectionView!, layout: self, widthForSectionAtColumn: i)
                self.columnItemWidths.append(columnWidth)
            }
        }
        guard self.columnItemWidths.count != 0 else { // 无列宽
            print("error: 列宽columnWidths未设置")
            return false
        }
        /*
        * 5 行列间隔
        */
        // 行已占用列宽
        var columnItemWidth: CGFloat = 0
        for itemWidth in self.columnItemWidths {
            columnItemWidth += itemWidth
        }
        // 相同行之间的间隔
        self.interitemSpacing = (self.collectionView!.bounds.size.width - self.sectionInset.left - self.sectionInset.right - columnItemWidth) / CGFloat(self.columnItemWidths.count-1)
        // 不同行之间的间隔和相同行之间的间隔一样，界面美观
        self.lineSpacing = self.interitemSpacing
        return true
    }
    
    // MARK: 更新header布局
    private func prepareLayoutSectionHeader(section: Int) {
        if self.headerReferenceHeight > 0 {
            let indexPath = NSIndexPath(forRow: 0, inSection: section)
            let attributes = UICollectionViewLayoutAttributes(forSupplementaryViewOfKind: UICollectionElementKindSectionHeader, withIndexPath: indexPath)
            attributes.frame.origin.y = self.flowHeight
            attributes.frame.size.height = self.headerReferenceHeight
            attributes.frame.size.width = self.collectionView!.bounds.size.width
            // 添加到数据源
            self.headersAttributes.append(attributes)
            self.allItemAttributes.append(attributes)
            self.flowHeight = CGRectGetMaxY(attributes.frame)
        }
    }
    
    // MARK: 更新cell布局
    private func prepareLayoutSectionCell(section: Int) {
        self.flowHeight += self.sectionInset.top // 调整显示高
        // 块拆分
        let numberOfItemsInSection = self.collectionView!.numberOfItemsInSection(section)
        var numberOfSectionItems = numberOfItemsInSection / self.sectionItemsCount
        if self.collectionView!.numberOfItemsInSection(section) % self.sectionItemsCount != 0 {
            numberOfSectionItems++
        }
        var sectionItemAttribute = [UICollectionViewLayoutAttributes]() // section中的Attribute
        var sectionItemsRect = CGRectZero // section中的item显示区域
        var attributes = UICollectionViewLayoutAttributes() // 添加的LayoutAttributes
        for numberOfSectionItem in 0..<numberOfSectionItems {
            var sectionItemsLeft = self.sectionInset.left // sectionItem的left
            for (indexSectionItem, sectionItem) in self.sectionItems.enumerate() {
                var sectionItemsTop = self.flowHeight // sectionItem的top
                // 处理小列
                for var item in sectionItem {
                    item = self.sectionItemsCount * numberOfSectionItem + item
                    guard item < numberOfItemsInSection else { // 安全处理
                        break
                    }
                    let indexPath = NSIndexPath(forItem: item, inSection: section)
                    // 组装UICollectionViewLayoutAttributes
                    attributes = UICollectionViewLayoutAttributes(forCellWithIndexPath: indexPath)
                    attributes.frame.origin.x = sectionItemsLeft
                    attributes.frame.origin.y = sectionItemsTop
                    attributes.frame.size.height = self.delegateFlowLayout!.collectionView(self.collectionView!, layout: self, heightForItemAtIndexPath: indexPath)
                    attributes.frame.size.width = self.columnItemWidths[indexSectionItem]
                    sectionItemAttribute.append(attributes) // 添加到临时数据源
                    sectionItemsTop = CGRectGetMaxY(attributes.frame) + self.lineSpacing // 移动top
                    sectionItemsRect = CGRectUnion(sectionItemsRect, attributes.frame) // 扩大section中的item显示区域
                }
                sectionItemsLeft = CGRectGetMaxX(attributes.frame) + self.interitemSpacing // 移动left
            }
            self.flowHeight = CGRectGetMaxY(sectionItemsRect) + self.lineSpacing // 移动高度显示
        }
        // 添加到数据源
        self.sectionItemAttributes.append(sectionItemAttribute)
        self.allItemAttributes.appendContentsOf(sectionItemAttribute)
        self.flowHeight = self.flowHeight - self.lineSpacing + self.sectionInset.bottom // 移动高度显示
    }
    
    // MARK: 更新footer布局
    private func prepareLayoutSectionFooter(section: Int) {
        if self.footerReferenceHeight > 0 {
            let indexPath = NSIndexPath(forRow: 0, inSection: section)
            let attributes = UICollectionViewLayoutAttributes(forSupplementaryViewOfKind: UICollectionElementKindSectionFooter, withIndexPath: indexPath)
            attributes.frame.origin.y = self.flowHeight
            attributes.frame.size.height = self.footerReferenceHeight
            attributes.frame.size.width = self.collectionView!.bounds.size.width
            // 添加到数据源
            self.footersAttributes.append(attributes)
            self.allItemAttributes.append(attributes)
            self.flowHeight = CGRectGetMaxY(attributes.frame)
        }
    }
    
}
```

#<a id="实战演练"/>3 实战演练

相关界面仿照前面的博文自行设计即可。这里展示YJFlowLayoutCollectionVC的所有源代码。

```swift
//
//  YJFlowLayoutCollectionVC.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/22.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

private let reuseIdentifier = "customCell"

/// 自定义UICollectionViewFlowLayout
class YJFlowLayoutCollectionVC: UICollectionViewController, YJCollectionViewDelegateFlowLayout {
    
    /// 数据源
    private var data = [[Int]]()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // 测试数据
        for _ in 0...2 {
            var list = [Int]()
            for i in 0..<50 {
                list.append(i)
            }
            self.data.append(list)
        }
        // 修改YJCollectionViewFlowLayout
        let layout = self.collectionView!.collectionViewLayout as! YJCollectionViewFlowLayout
        layout.headerReferenceHeight = 60
        layout.footerReferenceHeight = 50
        layout.sectionInset = UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 10)
        switch UIDevice.currentDevice().orientation{
        case .LandscapeLeft, .LandscapeRight:
            layout.sectionItems = [[0], [1,7], [2,8], [3,9], [4,10], [5,11], [6,12]]
        default:
            layout.sectionItems = [[0], [1,5], [2,6],[3,7],[4,8]]
        }
        // 监听设备方向
        NSNotificationCenter.defaultCenter().addObserver(self, selector: "receivedRotation",
            name: UIDeviceOrientationDidChangeNotification, object: nil)
        // 注册Cell
        let nib = UINib(nibName: "YJCollectionViewCell", bundle: nil)
        self.collectionView?.registerNib(nib, forCellWithReuseIdentifier: reuseIdentifier)
    }
    
    //通知监听触发的方法
    func receivedRotation(){
        let device = UIDevice.currentDevice()
        let layout = self.collectionView!.collectionViewLayout as! YJCollectionViewFlowLayout
        switch device.orientation{
        case UIDeviceOrientation.Portrait, UIDeviceOrientation.PortraitUpsideDown:
            layout.sectionItems = [[0], [1,5], [2,6],[3,7],[4,8]]
        case .LandscapeLeft, .LandscapeRight:
            layout.sectionItems = [[0], [1,7], [2,8], [3,9], [4,10], [5,11], [6,12]]
        default:
            print(device.orientation)
        }
    }
    
    // MARK: - YJCollectionViewDelegateFlowLayout
    func collectionView(collectionView: UICollectionView, layout collectionViewFlowLayout: YJCollectionViewFlowLayout, widthForSectionAtColumn column: Int) -> CGFloat {
        // 如一行显示3个间隔设置为10，则公式为(2width+10)+2width+20 = collectionView.bounds.size.width - sectionInset.left - sectionInset.right
        let sectionItemCount: CGFloat = CGFloat(collectionViewFlowLayout.sectionItems.count)
        let width = (collectionView.bounds.size.width - collectionViewFlowLayout.sectionInset.left - collectionViewFlowLayout.sectionInset.right - (sectionItemCount-1)*10) / (sectionItemCount+1)
        if column == 0 {
            return 2 * width + 10
        }
        return width
    }
    
    func collectionView(collectionView: UICollectionView, layout collectionViewFlowLayout: YJCollectionViewFlowLayout, heightForItemAtIndexPath indexPath: NSIndexPath) -> CGFloat {
        let sectionItemCount: CGFloat = CGFloat(collectionViewFlowLayout.sectionItems.count)
        let height = (collectionView.bounds.size.width - collectionViewFlowLayout.sectionInset.left - collectionViewFlowLayout.sectionInset.right - (sectionItemCount-1)*10) / (sectionItemCount+1)
        if indexPath.row % collectionViewFlowLayout.sectionItemsCount == 0 {
            return 2 * height + 10
        }
        return height
    }
    
    // MARK: - UICollectionViewDataSource
    override func numberOfSectionsInCollectionView(collectionView: UICollectionView) -> Int {
        return self.data.count
    }
    
    override func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return self.data[section].count
    }
    
    override func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCellWithReuseIdentifier(reuseIdentifier, forIndexPath: indexPath) as! YJCollectionViewCell
        cell.backgroundColor = UIColor.grayColor()
        cell.textLabel.text = "\(indexPath.item)"
        return cell
    }
    
    override func collectionView(collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionReusableView {
        var crView: UICollectionReusableView!
        if (kind == UICollectionElementKindSectionHeader) { // Header
            crView = collectionView.dequeueReusableSupplementaryViewOfKind(kind, withReuseIdentifier: "header", forIndexPath: indexPath)
            // 标题
            if let label: UILabel = crView.viewWithTag(8) as? UILabel {
                label.text = "\(indexPath.section) Section"
            }
            crView.backgroundColor = UIColor.orangeColor()
        } else { // Footer
            crView = collectionView.dequeueReusableSupplementaryViewOfKind(kind, withReuseIdentifier: "footer", forIndexPath: indexPath)
            crView.backgroundColor = UIColor.purpleColor()
        }
        return crView
    }
    
}
```

运行项目可看见如下效果图。

竖屏效果图:

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015122309.jpg)

横屏效果图:

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015122310.jpg)

&#160;

---

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UICollectionView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/index.html)

[UICollectionViewDataSource Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewDataSource_protocol/index.html)

[UICollectionReusableView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionReusableView_class/index.html)

[UICollectionViewCell Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewCell_class/index.html)

[UICollectionViewLayout Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewLayout_class/index.html)

[UICollectionViewFlowLayout Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewFlowLayout_class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-24 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog