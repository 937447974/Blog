[UICollectionViewDataSource](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDataSource.md)

[UICollectionViewDelegate](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegate.md)

[UICollectionViewDelegateFlowLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegateFlowLayout.md)

[UICollectionViewCell](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewCell.md)

[UICollectionViewLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewLayout.md)

---

#1 UICollectionViewDelegateFlowLayout

UICollectionViewDelegateFlowLayout是UICollectionViewDelegate的子类，也就是说它继承了UICollectionViewDelegate的所有协议。在UICollectionViewDelegate的基础上，UICollectionViewDelegateFlowLayout增加了关于UICollectionViewFlowLayout的相关控制，让我们能够更加精细的控制视图的布局。

##1.1 获取Item的Size

```swift
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAtIndexPath indexPath: NSIndexPath) -> CGSize
```

##1.2 获取Section的间隔

```swift
// 边间隔
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, insetForSectionAtIndex section: Int) -> UIEdgeInsets
    
// MARK: 行间隔
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumLineSpacingForSectionAtIndex section: Int) -> CGFloat
    
// MARK: 列间隔
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumInteritemSpacingForSectionAtIndex section: Int) -> CGFloat
```

##1.3 获取Header和Footer的Size

```swift
// MARK: Header显示
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, referenceSizeForHeaderInSection section: Int) -> CGSize
    
// MARK: Footer显示
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, referenceSizeForFooterInSection section: Int) -> CGSize
```

#2 实战演练

##2.1 界面搭建

界面搭建如[UICollectionViewDataSource](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDataSource.md)所示，这里不在详细解锁。

##2.2 类YJCollectionViewDelegateFlowLayoutVC

这里使用类YJCollectionViewDelegateFlowLayoutVC为搭建演示关于UICollectionViewDelegateFlowLayout的使用，下面是部分源代码。

```swift
//
//  YJCollectionViewDelegateFlowLayoutVC.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/21.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// UICollectionViewDelegateFlowLayout
class YJCollectionViewDelegateFlowLayoutVC: UIViewController, UICollectionViewDataSource, UICollectionViewDelegateFlowLayout {

    /// UICollectionView
    @IBOutlet weak var collectionView: UICollectionView!
    /// 数据源
    private var data = [[Int]]()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // 测试数据
        for _ in 0...2 {
            var list = [Int]()
            for i in 0..<10 {
                list.append(i)
            }
            self.data.append(list)
        }
        // 长点击事件，做移动cell操作
        self.collectionView.allowsMoveItem()
    }
    
    // MARK: - UICollectionViewDataSource
    func numberOfSectionsInCollectionView(collectionView: UICollectionView) -> Int {
        return self.data.count
    }
    
    func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return self.data[section].count
    }
    
    func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCellWithReuseIdentifier("cell", forIndexPath: indexPath)
        cell.backgroundColor = UIColor.grayColor()
        if let label: UILabel = cell.viewWithTag(8) as? UILabel {
            label.text = "\(indexPath.item)"
        }
        return cell
    }
    
    func collectionView(collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionReusableView {
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

##2.3 实现UICollectionViewDelegateFlowLayout

接下来实现UICollectionViewDelegateFlowLayout。

```swift
// MARK: - UICollectionViewDelegateFlowLayout
// MARK: - Getting the Size of Items
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAtIndexPath indexPath: NSIndexPath) -> CGSize {
    print(__FUNCTION__)
    // 每一行显示5个
    let weight = (YJUtilScreenSize.screenMinLength-10*5)/4
    return CGSize(width: weight, height: weight)
}
    
// MARK: - Getting the Section Spacing
// 边间隔
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, insetForSectionAtIndex section: Int) -> UIEdgeInsets {
    print(__FUNCTION__)
    return UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 10)
}
    
// MARK: 行间隔
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumLineSpacingForSectionAtIndex section: Int) -> CGFloat {
    print(__FUNCTION__)
    return 10
}
    
// MARK: 列间隔
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumInteritemSpacingForSectionAtIndex section: Int) -> CGFloat {
    print(__FUNCTION__)
    return 10
}
    
// MARK: - Getting the Header and Footer Sizes
// MARK: Header显示
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, referenceSizeForHeaderInSection section: Int) -> CGSize {
    print(__FUNCTION__)
    return CGSize(width: collectionView.frame.width, height: 50)
}
    
// MARK: Footer显示
func collectionView(collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, referenceSizeForFooterInSection section: Int) -> CGSize {
    print(__FUNCTION__)
    return CGSize(width: collectionView.frame.width, height: 50)
}
```

运行项目后，看见如下效果图。


![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)



&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UICollectionView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/index.html)

[UICollectionViewDataSource Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewDataSource_protocol/index.html)

[UICollectionViewDelegateFlowLayout Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewDelegateFlowLayout_protocol/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-23 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog