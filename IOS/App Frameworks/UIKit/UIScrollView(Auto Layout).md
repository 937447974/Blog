[UIScrollView(UIScrollViewDelegate)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIScrollView(UIScrollViewDelegate).md)

[UIScrollView(Auto Layout)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIScrollView(Auto%20Layout).md)

---

在工作中使用UIScrollView的时候我们通常会做适配，也就是Auto Layout。这里我会使用纯代码和Storyboard界面做适配。

适配的核心就是约束，对UIScrollView做适配时要记住，对外的约束是显示区域，对内的约束是滑动区域。我总结为八个字：对外显示对内滑动。

# 1 纯代码适配

```swift
//
//  YJAutoLayoutSVVC.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/17.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

class YJAutoLayoutSVVC: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        self.title = "纯代码AutoLayout"
        // 创建UIScrollView和UIImageView
        let scrollView  = UIScrollView()
        let imageView = UIImageView()
        imageView.image = UIImage(named: "ScrollViewBigImage")
        
        // 添加视图
        self.view.addSubview(scrollView)
        scrollView.addSubview(imageView)
        
        // 开启AutoLayout
        scrollView.translatesAutoresizingMaskIntoConstraints  = false;
        imageView.translatesAutoresizingMaskIntoConstraints = false;
        
        // self.view和ScrollView的约束
        // 显示区域,方式1添加约束
        let centerXC = NSLayoutConstraint(item: scrollView, attribute: NSLayoutAttribute.CenterX, relatedBy: NSLayoutRelation.Equal, toItem: self.view, attribute: NSLayoutAttribute.CenterX, multiplier: 1, constant: 0) // 中心x点对齐
        let centerYC = NSLayoutConstraint(item: scrollView, attribute: .CenterY, relatedBy: .Equal, toItem: self.view, attribute: .CenterY, multiplier: 1, constant: 0) // 中心y点对齐
        self.view.addConstraints([centerXC, centerYC]) // 组添加
        let heightC = NSLayoutConstraint(item: scrollView, attribute: .Height, relatedBy: .Equal, toItem: self.view, attribute: .Height, multiplier: 1, constant: 0) // 等高
        self.view.addConstraint(heightC) // 单一添加
        NSLayoutConstraint(item: scrollView, attribute: .Width, relatedBy: .Equal, toItem: self.view, attribute: .Width, multiplier: 1, constant: 0).active = true // 等宽，立即执行
        
        // 滑动区域,方式2添加约束
        // NSLayoutYAxisAnchor封装NSLayoutConstraint写法
        scrollView.topAnchor.constraintEqualToAnchor(imageView.topAnchor).active = true // Top
        scrollView.bottomAnchor.constraintEqualToAnchor(imageView.bottomAnchor).active = true // Bottom
        scrollView.leadingAnchor.constraintEqualToAnchor(imageView.leadingAnchor).active = true // leading
        scrollView.trailingAnchor.constraintEqualToAnchor(imageView.trailingAnchor).active = true // trailing
    }

}
```

运行效果图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015121804.jpg)

这里为大家展示了三种约束效果。

# 2 Storyboard约束

Storyboard约束讲解起来比较麻烦，就用一个视屏为大家演示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015121803.gif)

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[UIScrollView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIScrollView_Class/index.html)

[UIScrollView And Autolayout](https://developer.apple.com/library/ios/technotes/tn2154/_index.html)

[Auto Layout Guide](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-18 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog