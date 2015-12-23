[UICollectionViewDataSource](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDataSource.md)

[UICollectionViewDelegate](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegate.md)

[UICollectionViewDelegateFlowLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegateFlowLayout.md)

[UICollectionViewCell](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewCell.md)

[UICollectionViewLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewLayout.md)

---

---

#1 UICollectionViewCell

UICollectionViewCell就是UICollectionView显示的一个一个元素，你可以通过它定制各种精美的界面。原始样式很简单，就是常用的View。多数情况在开发中我们会自定义各种各样的通用Cell供其他人使用，这篇博文也会介绍自定义Cell。

##1.1 Cell中的View

```
/// 自定义控件的父View
var contentView: UIView { get }
/// 背景
var backgroundView: UIView?
/// 选中时的背景
var selectedBackgroundView: UIView?
```

##1.2 管理Cell的状态

```swift
/// 是否选中
var selected: Bool
/// 是否高亮
var highlighted: Bool
```

#2 自定义UICollectionViewCell

我们知道UICollectionViewCell是UICollectionReusableView的子类，而UICollectionReusableView是UICollectionView的Header和Footer。也就意味着自定义UICollectionViewCell的方式也同样适用于自定义Header和Footer。

多数情况下，我们开发过程中自定义UICollectionViewCell，不是在主VC界面直接创建的，而是通过xib或者纯代码创建。这样有利于公用，以及维护。

##2.1 创建xib



![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UICollectionView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/index.html)

[UICollectionViewDataSource Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewDataSource_protocol/index.html)

[UICollectionReusableView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionReusableView_class/index.html)

[UICollectionViewCell Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionViewCell_class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-23 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog