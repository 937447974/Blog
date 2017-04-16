1. [搭建项目](#搭建项目)
2. [展示照片](#展示照片)
3. [移动照片](#移动照片)
4. [缩放照片](#缩放照片)

---

## <a id="搭建项目"/>1 搭建项目

本次讲解使用纯代码的方式向大家演示怎么使用UIScrollView。这里使用了类YJScrollViewDelegateVC.swift

```swift
//
//  YJScrollViewDelegateVC.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/17.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// UIScrollViewDelegate
class YJScrollViewDelegateVC: UIViewController, UIScrollViewDelegate {
    
    /// 照片
    var imageView: UIImageView!
    /// UIScrollView
    var scrollView: UIScrollView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        self.title = "纯代码"
        
        let scrollItem = UIBarButtonItem(title: "Scroll", style: .Plain, target: self, action: "onClickScroll")
        let zoomItem = UIBarButtonItem(title: "Zoom", style: .Plain, target: self, action: "onClickZoom")
        self.navigationItem.rightBarButtonItems = [scrollItem, zoomItem]
    }
    
}
```

运行项目可看见如下效果图。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015121801.jpg)

## <a id="展示照片"/>2 展示照片

我们使用一张照片为大家演示缩放和移动功能，这里显示照片。

在viewDidLoad()方法中添加如下代码

```swift
// 填充UIScrollView
self.scrollView = UIScrollView(frame: self.view.frame)
self.view.addSubview(self.scrollView)
self.scrollView.delegate = self
// 图片
let image = UIImage(named: "ScrollViewBigImage")
self.imageView = UIImageView(image: image)
self.scrollView.addSubview(self.imageView)
self.scrollView.contentSize = image!.size // 可移动区域
```

> 这里设置了contentSize属性，照片才可以移动。

运行项目看见如下效果图。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015121802.jpg)

此时你已经可以移动照片了。

## <a id="移动照片"/>3 移动照片

我们还可以通过代码动态移动照片。

实现onClickScroll()方法。

```swift
// MARK: 移动
func onClickScroll() {
    print(__FUNCTION__)
    var point = self.scrollView.contentOffset // 当前点
    point.x += 100 // 向左移动100
    point.x = point.x >= self.scrollView.contentSize.width ? 0 : point.x
    self.scrollView.setContentOffset(point, animated: true)// 动画移动
}
```

运行项目后点击界面的Scroll，即可看见照片动画左移。

## <a id="缩放照片"/>4 缩放照片

缩放照片会负责一点，需要借助UIScrollViewDelegate代理。

### 4.1 设置缩放比例

设置缩放的最大和最小比例，在viewDidLoad()方法中添加如下代码

```swift
// 缩放
self.scrollView.minimumZoomScale = 0.5
self.scrollView.maximumZoomScale = 2
```

### 4.2 实现代理

UIScrollView并不知道你要缩放那个View，故需要实现代理方法。

```swift
// MARK: 返回要缩放的View
func viewForZoomingInScrollView(scrollView: UIScrollView) -> UIView?  {
    print(__FUNCTION__)
    return self.imageView
}
```

### 4.3 动画缩放

UIScrollView还支持动画缩放View。

实现onClickZoom()方法。

```swift
// MARK: 缩放
func onClickZoom() {
    print(__FUNCTION__)
    var zoomScale = self.scrollView.zoomScale // 当前缩放
    zoomScale += 0.5
    if zoomScale >= self.scrollView.maximumZoomScale {
        zoomScale = self.scrollView.minimumZoomScale
    }
    self.scrollView.setZoomScale(zoomScale, animated: true) // 动画缩放
}
```

点击界面的Zoom按钮即可看见缩放动画。

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[UIScrollView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIScrollView_Class/index.html)

[UIScrollViewDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIScrollViewDelegate_Protocol/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-18 | 博文完成 |
| 2016-07-25 | 博文修改，只展示实战演练，其他模块分离成新文档。 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog