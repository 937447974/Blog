[UICollectionViewDataSource](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDataSource.md)

[UICollectionViewDelegate](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegate.md)

[UICollectionViewDelegateFlowLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegateFlowLayout.md)

[UICollectionViewCell](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewCell.md)

[UICollectionViewLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewLayout.md)

---

#1 UICollectionViewDataSource

UICollectionViewDataSource是UICollectionView的数据源协议，管理UICollectionView显示用的相关视图配置。

##1.1 获取Item和Section数量

```swift
// MARK: 分组
optional func numberOfSectionsInCollectionView(collectionView: UICollectionView) -> Int
    
// MARK: 每一组有多少个cell
func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int
```

##1.2 获取显示的视图

```swift
// MARK: 生成Cell
func collectionView(_ collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell

// MARK: 生成Header或Footer
optional func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionReusableView
```

##1.3 移动Item

```swift
// MARK: 能否移动
optional func collectionView(_ collectionView: UICollectionView, canMoveItemAtIndexPath indexPath: NSIndexPath) -> Bool

// MARK: 移动cell
collectionView(_:moveItemAtIndexPath:toIndexPath:)
```

#2 实战演练

##2.1 项目搭建

这里使用storyboard搭建项目。主布局如图所示

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015122301.png)

侧边栏

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015122302.png)

这里我填充了一个header、footer和cell的布局，并设置相关Identifier属性。便于缓存界面。

设置Collection View Flow Layout

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015122303.png)

这里主要控制界面的相关布局。

##2.2 源代码

这里使用了类

```swift
//
//  YJCollectionViewDataSourceVC.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/19.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// UICollectionViewDataSource
class YJCollectionViewDataSourceVC: UIViewController, UICollectionViewDataSource {

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

}
```

这里只是填充了相关测试数据，其中`self.collectionView.allowsMoveItem()`是一个自定义的扩展方法，主要用于手势移动cell。

##2.3 实现UICollectionViewDataSource

接下来我们就实现UICollectionViewDataSource让主界面显示相关视图。

```swift
// MARK: - UICollectionViewDataSource
// MARK: 分组
func numberOfSectionsInCollectionView(collectionView: UICollectionView) -> Int {
    return self.data.count
}
    
// MARK: 每一组有多少个cell
func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    return self.data[section].count
}
    
// MARK: - Getting Views for Items
// MARK: 生成Cell
func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {
    let cell = collectionView.dequeueReusableCellWithReuseIdentifier("cell", forIndexPath: indexPath)
    cell.backgroundColor = UIColor.grayColor()
    if let label: UILabel = cell.viewWithTag(8) as? UILabel {
        label.text = "\(indexPath.item)"
    }
    return cell
}
    
// MARK: 生成Header或Footer
func collectionView(collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionReusableView {
    print(__FUNCTION__)
    var crView: UICollectionReusableView!
    if (kind == UICollectionElementKindSectionHeader) { // Header
        crView = collectionView.dequeueReusableSupplementaryViewOfKind(kind, withReuseIdentifier: "header", forIndexPath: indexPath)
        // 标题
        if let label: UILabel = crView.viewWithTag(8) as? UILabel {
            label.text = "\(indexPath.section) Section"
        }
        crView.backgroundColor = UIColor.redColor()
    } else { // Footer
        crView = collectionView.dequeueReusableSupplementaryViewOfKind(kind, withReuseIdentifier: "footer", forIndexPath: indexPath)
        crView.backgroundColor = UIColor.greenColor()
    }
    return crView
}
    
// MARK: - Reordering Items
// MARK: 能否移动
func collectionView(collectionView: UICollectionView, canMoveItemAtIndexPath indexPath: NSIndexPath) -> Bool {
    print(__FUNCTION__)
    return true
}
    
// MARK: 移动cell
func collectionView(collectionView: UICollectionView, moveItemAtIndexPath sourceIndexPath: NSIndexPath, toIndexPath destinationIndexPath: NSIndexPath) {
    print(__FUNCTION__)
    print(sourceIndexPath)
    print(destinationIndexPath)
    // 修改数据源
    let temp = self.data[sourceIndexPath.section].removeAtIndex(sourceIndexPath.item)
    self.data[destinationIndexPath.section].insert(temp, atIndex: destinationIndexPath.item)
    print(self.data)
}
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[NSURLSession](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSURLSession_class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-03 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog