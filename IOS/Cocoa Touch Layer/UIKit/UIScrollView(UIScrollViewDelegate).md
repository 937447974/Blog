[UIScrollView(UIScrollViewDelegate)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIScrollView(UIScrollViewDelegate).md)

[UIScrollView(Auto Layout)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIScrollView(Auto%20Layout).md)

---

#1 UIScrollView 

UIScrollView是一个很强大的类，它能提供比应用程序更大的空间给用户查看。如一张很大的图片，用户即可在手机上移动和捏合查看图片。

UIScrollView向下延生了三个子类UICollectionView、UITableView和UITextView。这都是我们工作中常用的View。

##1.1 管理内容的显示

```swift
// 动态设置原点，即移动
public func setContentOffset(_ contentOffset: CGPoint, animated animated: Bool)
// 原点所对应的contentview的坐标
public var contentOffset: CGPoint
// 可移动的区域
public var contentSize: CGSize
// contentview和边的距离
public var contentInset: UIEdgeInsets 
```

##1.2 管理滚动

```swift
// 能否滚动
public var scrollEnabled: Bool
// 锁定某个方向的滚动
public var directionalLockEnabled: Bool
// 能否自动回到顶部
public var scrollsToTop: Bool
// 可视区域
public func scrollRectToVisible(rect: CGRect, animated: Bool)
// 以页的形式滚动
public var pagingEnabled: Bool
// 全局滚动阻力
public var bounces: Bool 
// y轴滚动阻力
public var alwaysBounceVertical: Bool
// x轴滚动阻力
public var alwaysBounceHorizontal: Bool
// 滚动的速度
public var decelerationRate: CGFloat
// 用户是否触摸屏幕
public var tracking: Bool { get } 
// 是否在滚动
public var dragging: Bool { get }
// 是否在减速动画
public var decelerating: Bool { get }
```

##1.3 管理滚动条

```swift
// 滚动条的样式
public var indicatorStyle: UIScrollViewIndicatorStyle
// 滚动条的位置
public var scrollIndicatorInsets: UIEdgeInsets
// 是否显示x轴滚动条
public var showsHorizontalScrollIndicator: Bool
// 是否显示y轴滚动条
public var showsVerticalScrollIndicator: Bool
// 随时显示滚动条
public func flashScrollIndicators()
```

##1.4 缩放和平移

```swift
// 缩放的最小比例
public var minimumZoomScale: CGFloat
// 缩放的最大比例
public var maximumZoomScale: CGFloat 
// 当前缩放比例
public var zoomScale: CGFloat // default is 1.0
// 是否动画设置缩放
public func setZoomScale(scale: CGFloat, animated: Bool)
// 设置缩放区域
public func zoomToRect(rect: CGRect, animated: Bool)
//缩放阻力效果  
public var bouncesZoom: Bool 
// 用户是否在进行缩放操作
public var zooming: Bool { get }
// 是否有操作指定的缩放
public var zoomBouncing: Bool { get }
```

#2 UIScrollViewDelegate

UIScrollViewDelegate是UIScrollView的核心代理，我们可以通过它监听UIScrollView的相关动作。

##2.1 响应滚动

```swift
// MARK: - 滚动持续中
func scrollViewDidScroll(scrollView: UIScrollView)
    
// MARK: - 手指触摸开始滚动
func scrollViewWillBeginDragging(scrollView: UIScrollView)
    
// MARK: 手指离开屏幕后，移动速度
func scrollViewWillEndDragging(scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>)
    
// MARK: 手指离开屏幕后，是否有减速动画
func scrollViewDidEndDragging(scrollView: UIScrollView, willDecelerate decelerate: Bool)
    
// MARK: 手指离开屏幕后，滚动视图滚动开始减速动画
func scrollViewWillBeginDecelerating(scrollView: UIScrollView)
    
// MARK: 手指移动引起的滚动动画结束
func scrollViewDidEndDecelerating(scrollView: UIScrollView)
    
// MARK: 使用setContentOffset/scrollRectVisible:animated:调用的滚动动画结束
func scrollViewDidEndScrollingAnimation(scrollView: UIScrollView)

```

##2.2 管理缩放

```swift
// MARK: 缩放持续中
optional public func scrollViewDidZoom(scrollView: UIScrollView)

// MARK: 返回要缩放的View
optional public func viewForZoomingInScrollView(scrollView: UIScrollView) -> UIView?  
    
// MARK: 开始缩放
optional public func scrollViewWillBeginZooming(scrollView: UIScrollView, withView view: UIView?)

// MARK: 缩放停止
optional public func scrollViewDidEndZooming(scrollView: UIScrollView, withView view: UIView?, atScale scale: CGFloat)
```

##2.3 回到顶部

```swift
// MARK: 点击顶部的时间，是否回滚到顶部
optional public func scrollViewShouldScrollToTop(scrollView: UIScrollView) -> Bool
    
// MARK: 回滚到顶部结束
optional public func scrollViewDidScrollToTop(scrollView: UIScrollView)
```

#3 实战演练

##3.1 搭建项目

本次讲解使用纯代码的方式向大家演示怎么使用UIScrollView。这里使用了类

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





![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UIScrollView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIScrollView_Class/index.html)

[UIScrollViewDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIScrollViewDelegate_Protocol/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-03 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog