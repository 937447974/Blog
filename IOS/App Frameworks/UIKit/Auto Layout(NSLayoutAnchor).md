[Auto Layout(NSLayoutConstraint)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/Auto%20Layout(NSLayoutConstraint).md)

[Auto Layout(NSLayoutAnchor)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/Auto%20Layout(NSLayoutAnchor).md)

[Auto Layout(Storyboard)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/Auto%20Layout(Storyboard).md)

---

**1 [NSLayoutAnchor](#NSLayoutAnchor)**

1. [对比NSLayoutConstraint和NSLayoutAnchor](#对比NSLayoutConstraint和NSLayoutAnchor)
2. [ 创建NSLayoutAnchor](#创建NSLayoutAnchor)

**2 [NSLayoutAnchor的子类](#NSLayoutAnchor的子类)**

1. [相关子类](#相关子类)
2. [获取子类](#获取子类)

**3 [实战演练](#实战演练)**

1. [效果图模型](#效果图模型)
2. [代码实现](#代码实现)
3. [效果图](#效果图)

---

#<a id="NSLayoutAnchor"/>1 NSLayoutAnchor

前面讲到了NSLayoutConstraint约束UI界面。但是通过实际操作，这样的写法特别繁琐特别麻烦。今天向大家介绍NSLayoutAnchor，这种写法很简洁大方。它会生成一个NSLayoutConstraint供你使用。

> NSLayoutAnchor是IOS9推出的。

##<a id="对比NSLayoutConstraint和NSLayoutAnchor"/>1.1 对比NSLayoutConstraint和NSLayoutAnchor

还记得NSLayoutConstraint创建约束时，如下所示。

```swift
NSLayoutConstraint(item: subview,
    attribute: .Leading,
    relatedBy: .Equal,
    toItem: view,
    attribute: .LeadingMargin,
    multiplier: 1.0,
    constant: 0.0).active = true
```

然后我们用NSLayoutAnchor创建同样的约束。

```swift
subview.leadingAnchor.constraintEqualToAnchor(margins.leadingAnchor).active = true
```

一对比是否感觉使用NSLayoutAnchor更酸爽。

##<a id="创建NSLayoutAnchor"/>1.2 创建NSLayoutAnchor

使用创建约束有如下几种方式

```swift
/* These methods return an inactive constraint of the form thisAnchor = otherAnchor.
*/
public func constraintEqualToAnchor(anchor: NSLayoutAnchor!) -> NSLayoutConstraint!
public func constraintGreaterThanOrEqualToAnchor(anchor: NSLayoutAnchor!) -> NSLayoutConstraint!
public func constraintLessThanOrEqualToAnchor(anchor: NSLayoutAnchor!) -> NSLayoutConstraint!
    
/* These methods return an inactive constraint of the form thisAnchor = otherAnchor + constant.
*/
public func constraintEqualToAnchor(anchor: NSLayoutAnchor!, constant c: CGFloat) -> NSLayoutConstraint!
public func constraintGreaterThanOrEqualToAnchor(anchor: NSLayoutAnchor!, constant c: CGFloat) -> NSLayoutConstraint!
public func constraintLessThanOrEqualToAnchor(anchor: NSLayoutAnchor!, constant c: CGFloat) -> NSLayoutConstraint!
```

创建NSLayoutAnchor约束的口诀和创建NSLayoutConstraint的口诀是一样的，都是“前右下后左上”。即

1. 左边的View对应self，右边的View对应anchor；
2. 下面的View对应self，上面的View对应anchor。

#<a id="NSLayoutAnchor的子类"/>2 NSLayoutAnchor的子类

##<a id="相关子类"/>2.1 相关子类

多数情况下，我们设置约束时是操作NSLayoutAnchor的子类。

1. NSLayoutYAxisAnchor: Y轴约束。
2. NSLayoutXAxisAnchor：X轴约束。
3. NSLayoutDimension：界面约束，如宽和高。

这里不在详细介绍，开发过程中看看API就知道。

##<a id="获取子类"/>2.2 获取子类

苹果对view进行了扩展，便于大家设置约束的时候，获取NSLayoutAnchor的子类。

```swift
extension UIView {
    /* Constraint creation conveniences. See NSLayoutAnchor.h for details.
     */
    @available(iOS 9.0, *)
    public var leadingAnchor: NSLayoutXAxisAnchor { get }
    @available(iOS 9.0, *)
    public var trailingAnchor: NSLayoutXAxisAnchor { get }
    @available(iOS 9.0, *)
    public var leftAnchor: NSLayoutXAxisAnchor { get }
    @available(iOS 9.0, *)
    public var rightAnchor: NSLayoutXAxisAnchor { get }
    @available(iOS 9.0, *)
    public var topAnchor: NSLayoutYAxisAnchor { get }
    @available(iOS 9.0, *)
    public var bottomAnchor: NSLayoutYAxisAnchor { get }
    @available(iOS 9.0, *)
    public var widthAnchor: NSLayoutDimension { get }
    @available(iOS 9.0, *)
    public var heightAnchor: NSLayoutDimension { get }
    @available(iOS 9.0, *)
    public var centerXAnchor: NSLayoutXAxisAnchor { get }
    @available(iOS 9.0, *)
    public var centerYAnchor: NSLayoutYAxisAnchor { get }
    @available(iOS 9.0, *)
    public var firstBaselineAnchor: NSLayoutYAxisAnchor { get }
    @available(iOS 9.0, *)
    public var lastBaselineAnchor: NSLayoutYAxisAnchor { get }
}
```

#<a id="实战演练"/>3 实战演练

##<a id="效果图模型">3.1 效果图模型

我们要实现和NSLayoutConstraint实战演练一样的效果图。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015121805.png)

一个黄View和一个绿View在不同的屏幕上显示同样的效果。

通过观察我们写出如下伪代码。

1. Yellow View.Leading = Superview.Leading + 20.0
2. Yellow View.Top = Top Layout Guide.Bottom + 20.0
3. Bottom Layout Guide.Top = Yellow View.Bottom + 20.0
4. Green View.Trailing = Superview.Trailing + 20.0
5. Green View.Top = Top Layout Guide.Bottom + 20.0
6. Bottom Layout Guide.Top = Green View.Bottom + 20.0
7. Green View.Leading = Yellow View.Trailing + 30.0
8. Yellow View.Width = Green View.Width

##<a id="代码实现">3.2 代码实现

在代码实现的时候，UIView是默认禁止约束的，你要通过。

```swift
public var translatesAutoresizingMaskIntoConstraints: Bool // Default YES
```

将该属性设为false时，则代表启用约束。

下面是核心代码的实现。

```swift
//
//  YJAutoLayoutAnchorVC.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/18.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// NSLayoutAnchor 是IOS9推出的，优化NSLayoutConstraint
class YJAutoLayoutAnchorVC: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 1 添加View
        // 黄View
        let yellowView = UIView()
        yellowView.backgroundColor = UIColor.yellowColor()
        self.view.addSubview(yellowView)
        // 绿View
        let greenView = UIView()
        greenView.backgroundColor = UIColor.greenColor()
        self.view.addSubview(greenView)
        
        // 2 开启AutoLayout
        yellowView.translatesAutoresizingMaskIntoConstraints  = false;
        greenView.translatesAutoresizingMaskIntoConstraints = false;
        
        // 3 设置约束
        /* 约束伪代码
        Yellow View.Leading = Superview.Leading + 20.0
        Yellow View.Top = Top Layout Guide.Bottom + 20.0
        Bottom Layout Guide.Top = Yellow View.Bottom + 20.0
        
        Green View.Trailing = Superview.Trailing + 20.0
        Green View.Top = Top Layout Guide.Bottom + 20.0
        Bottom Layout Guide.Top = Green View.Bottom + 20.0
        
        Green View.Leading = Yellow View.Trailing + 30.0
        Yellow View.Width = Green View.Width
        */
        // 3.1 yellow约束
        yellowView.leadingAnchor.constraintEqualToAnchor(self.view.leadingAnchor, constant: 20).active = true
        yellowView.topAnchor.constraintEqualToAnchor(self.topLayoutGuide.bottomAnchor, constant: 20).active = true
        self.bottomLayoutGuide.topAnchor.constraintEqualToAnchor(yellowView.bottomAnchor, constant: 20).active = true
        
        // 3.2 green约束
        greenView.topAnchor.constraintEqualToAnchor(self.topLayoutGuide.bottomAnchor, constant: 20).active = true
        self.view.trailingAnchor.constraintEqualToAnchor(greenView.trailingAnchor, constant: 20).active = true
        self.bottomLayoutGuide.topAnchor.constraintEqualToAnchor(greenView.bottomAnchor, constant: 20).active = true
        
        // 3.3 green和yellow的共有约束
        greenView.leadingAnchor.constraintEqualToAnchor(yellowView.trailingAnchor, constant: 30).active = true // 间距
        greenView.widthAnchor.constraintEqualToAnchor(yellowView.widthAnchor, constant: 20).active = true // 等宽
        
        // 打印所有约束
        for constraint in self.view.constraints {
            print(constraint)
        }
        greenView.widthAnchor.constraintEqualToConstant(<#T##c: CGFloat##CGFloat#>)
    }
    
}
```

##<a id="效果图">3.3 效果图

运行项目后，在不同的屏幕上都可以看到如下效果图，还可以旋转屏幕。

竖屏

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015121806.jpg)

横屏幕

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015121807.jpg)

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[NSLayoutAnchor Class Reference](https://developer.apple.com/library/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html)

[Auto Layout Guide](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-19 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog