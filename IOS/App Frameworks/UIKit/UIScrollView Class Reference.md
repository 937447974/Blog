UIScrollView是一个很强大的类，它能提供比手机界面更大的空间给用户查看。如一张很大的图片，用户即可在手机上移动和捏合查看图片。

UIScrollView向下延生了三个子类UICollectionView、UITableView和UITextView。这都是我们工作中常用的View。

##1 Managing the Display of Content

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

##2 Managing Scrolling

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
// 手势事件是否响应
public func touchesShouldBegin(touches: Set<UITouch>, withEvent event: UIEvent?, inContentView view: UIView) -> Bool
// 是否取消子视图
public func touchesShouldCancelInContentView(view: UIView) -> Bool
// 是否延迟处理触摸事件
public var delaysContentTouches: Bool
// 是否触摸导致跟踪
public var canCancelContentTouches: Bool
// 滚动的速度
public var decelerationRate: CGFloat
// 用户是否触摸屏幕
public var tracking: Bool { get } 
// 是否在滚动
public var dragging: Bool { get }
// 是否在减速动画
public var decelerating: Bool { get }
```

##3 Managing the Scroll Indicator

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

##4 Zooming and Panning

```swift
// UIPanGestureRecognizer手势
@available(iOS 5.0, *)
public var panGestureRecognizer: UIPanGestureRecognizer { get }
// UIPinchGestureRecognizer手势
@available(iOS 5.0, *)
public var pinchGestureRecognizer: UIPinchGestureRecognizer? { get }
// 是否缩放动画
@available(iOS 3.0, *)
public func zoomToRect(rect: CGRect, animated: Bool)
// 缩放比例
@available(iOS 3.0, *)
public var zoomScale: CGFloat
// 动画设置缩放比例
@available(iOS 3.0, *)
public func setZoomScale(scale: CGFloat, animated: Bool)
// 缩放的最小比例
public var minimumZoomScale: CGFloat
// 缩放的最大比例
public var maximumZoomScale: CGFloat 
//缩放阻力效果
public var bouncesZoom: Bool 
// 用户是否在进行缩放操作
public var zooming: Bool { get }
// 是否超过缩放指定的比例限制
public var zoomBouncing: Bool { get }
```

##5 Managing the Delegate

```swift
// 回调代理
weak public var delegate: UIScrollViewDelegate?
```

##6 Managing the Keyboard

```swift
// 键盘与滚动交互
@available(iOS 7.0, *)
public var keyboardDismissMode: UIScrollViewKeyboardDismissMode
```

&#160;

----------

#Appendix

##Related Documentation

[UIScrollView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIScrollView_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-07-25 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974