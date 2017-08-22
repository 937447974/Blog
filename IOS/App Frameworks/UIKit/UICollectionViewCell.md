[UICollectionViewDataSource](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDataSource.md)

[UICollectionViewDelegate](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegate.md)

[UICollectionViewDelegateFlowLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegateFlowLayout.md)

[UICollectionViewCell](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewCell.md)

[UICollectionViewLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewLayout.md)

---

**1 [UICollectionViewCell](#UICollectionViewCell)**

1. [Cell中的View](#Cell中的View)
2. [管理Cell的状态](#管理Cell的状态)

**2 [自定义UICollectionViewCell](#自定义UICollectionViewCell)**

1. [创建xib](#创建xib)
2. [配置xib](#配置xib)
3. [类YJCollectionViewCell](#类YJCollectionViewCell)

**3 [实战演练](#实战演练)**

---

# <a id="UICollectionViewCell"/>1 UICollectionViewCell

UICollectionViewCell就是UICollectionView显示的一个一个元素，你可以通过它定制各种精美的界面。原始样式很简单，就是常用的View。多数情况在开发中我们会自定义各种各样的通用Cell供其他人使用，这篇博文也会介绍自定义Cell。

## <a id="Cell中的View"/>1.1 Cell中的View

```
/// 自定义控件的父View
var contentView: UIView { get }
/// 默认背景
var backgroundView: UIView?
/// 选中时的背景
var selectedBackgroundView: UIView?
```

## <a id="管理Cell的状态"/>1.2 管理Cell的状态

```swift
/// 是否选中
var selected: Bool
/// 是否高亮
var highlighted: Bool
```

# <a id="自定义UICollectionViewCell"/>2 自定义UICollectionViewCell

我们知道UICollectionViewCell是UICollectionReusableView的子类，而UICollectionReusableView是UICollectionView的Header和Footer。也就意味着自定义UICollectionViewCell的方式也同样适用于自定义Header和Footer。

多数情况下，我们开发过程中自定义UICollectionViewCell，不是在主VC界面直接创建的，而是通过xib或者纯代码创建。这样有利于公用，以及维护。

这里我讲解通过xib自定义UICollectionViewCell，这也是我最喜欢的方式。

## <a id="创建xib"/>2.1 创建xib

创建类YJCollectionViewCell继承UICollectionViewCell，同时勾选"Also create XIB file"。这样同时创建类和xib，并且二者已经关联上了，无须我们做其他配置。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015122306.png)

## <a id="配置xib"/>2.2 配置xib

在YJCollectionViewCell.xib上UILable控件，如下所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015122307.png)

## <a id="类YJCollectionViewCell"/>2.3 类YJCollectionViewCell

类YJCollectionViewCell的源码如下所示。

```swift
//
//  YJCollectionViewCell.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/12.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// 自定义UICollectionViewCell
class YJCollectionViewCell: UICollectionViewCell {

    /// 显示内容
    @IBOutlet weak var textLabel: UILabel!
    
    override func awakeFromNib() {
        super.awakeFromNib()
        // Initialization code
    }

}
```

这样就定制了一个通用的UICollectionViewCell。

# <a id="实战演练"/>3 实战演练

为了简单点演示效果，这里我使用YJCollectionViewCellVC，并且继承UICollectionViewController。

```swift
//
//  YJCollectionViewCellVC.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/19.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// 自定义UICollectionViewCell
class YJCollectionViewCellVC: UICollectionViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        let nib = UINib(nibName: "YJCollectionViewCell", bundle: nil)
        self.collectionView?.registerNib(nib, forCellWithReuseIdentifier: "customCell")
    }
    
    override func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return 100
    }
    
    override func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCellWithReuseIdentifier("customCell", forIndexPath: indexPath) as! YJCollectionViewCell
        cell.backgroundColor = UIColor.grayColor()
        cell.textLabel.text = "\(indexPath.item)"
        return cell
    }

}
```

这里的核心是要使用UICollectionView`func registerNib(nib: UINib?, forCellWithReuseIdentifier identifier: String)`注册xib。这样有利于UICollectionViewCell的复用。

运行项目，能看到如下效果图：


![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015122308.jpg)

这样就自定义了UICollectionViewCell。相信看完后，你也能自定义你所需要的UICollectionViewCell。
&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[UICollectionView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/index.html)

[UICollectionViewDataSource Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewDataSource_protocol/index.html)

[UICollectionReusableView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionReusableView_class/index.html)

[UICollectionViewCell Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewCell_class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-23 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog