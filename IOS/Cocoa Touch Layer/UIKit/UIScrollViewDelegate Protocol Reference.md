UIScrollViewDelegate是UIScrollView的核心代理，我们可以通过它监听UIScrollView的相关动作。

##1 Responding to Scrolling and Dragging

```swift
// 滚动持续中
@available(iOS 2.0, *)
optional public func scrollViewDidScroll(scrollView: UIScrollView)
// 手指触摸开始滚动
@available(iOS 2.0, *)
optional public func scrollViewWillBeginDragging(scrollView: UIScrollView)
// 手指离开屏幕后，移动速度
@available(iOS 5.0, *)
optional public func scrollViewWillEndDragging(scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>)
// 手指离开屏幕后，是否有减速动画
@available(iOS 2.0, *)
optional public func scrollViewDidEndDragging(scrollView: UIScrollView, willDecelerate decelerate: Bool)
// 能否滚动到头部
@available(iOS 2.0, *)
optional public func scrollViewShouldScrollToTop(scrollView: UIScrollView) -> Bool
// 滚动到头部
@available(iOS 2.0, *)
optional public func scrollViewDidScrollToTop(scrollView: UIScrollView)
// 手指离开屏幕后，滚动视图滚动开始减速动画
func scrollViewWillBeginDecelerating(scrollView: UIScrollView)  
// 手指移动引起的滚动动画结束
func scrollViewDidEndDecelerating(scrollView: UIScrollView)
```

##2 Managing Zooming

```swift
// 返回要缩放的View
@available(iOS 2.0, *)
optional public func viewForZoomingInScrollView(scrollView: UIScrollView) -> UIView?
// 开始缩放
@available(iOS 3.2, *)
optional public func scrollViewWillBeginZooming(scrollView: UIScrollView, withView view: UIView?)
// 缩放持续中
@available(iOS 3.2, *)
optional public func scrollViewDidZoom(scrollView: UIScrollView)
// 缩放停止
@available(iOS 2.0, *)
optional public func scrollViewDidEndZooming(scrollView: UIScrollView, withView view: UIView?, atScale scale: CGFloat)
```

##3 Responding to Scrolling Animations

```swift
// 使用setContentOffset/scrollRectVisible:animated:调用的滚动动画结束
func scrollViewDidEndScrollingAnimation(scrollView: UIScrollView)
```

&#160;

----------

#Appendix

##Related Documentation

[UIScrollViewDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIScrollViewDelegate_Protocol/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-07-25 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974