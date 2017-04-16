UIPageViewControllerDelegate控制UIPageViewController的相关变化，如导航到新页面、书脊的位置等。

# Responding to Page View Controller Events

```swift
// 过渡动画开始
@available(iOS 6.0, *)
optional public func pageViewController(pageViewController: UIPageViewController, willTransitionToViewControllers pendingViewControllers: [UIViewController])
    
// 过渡动画结束
@available(iOS 5.0, *)
optional public func pageViewController(pageViewController: UIPageViewController, didFinishAnimating finished: Bool, previousViewControllers: [UIViewController], transitionCompleted completed: Bool)
    
// 返回书脊的位置
@available(iOS 5.0, *)
optional public func pageViewController(pageViewController: UIPageViewController, spineLocationForInterfaceOrientation orientation: UIInterfaceOrientation) -> UIPageViewControllerSpineLocation
```

# Overriding View Rotation Settings


```swift
// 支持的屏幕方向
@available(iOS 7.0, *)
optional public func pageViewControllerSupportedInterfaceOrientations(pageViewController: UIPageViewController) -> UIInterfaceOrientationMask
@available(iOS 7.0, *)

// 页面控制器的首选方向
optional public func pageViewControllerPreferredInterfaceOrientationForPresentation(pageViewController: UIPageViewController) -> UIInterfaceOrientation
```

&#160;

----------

# Appendix

## Related Documentation

[UIPageViewControllerDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIPageViewControllerDelegateProtocolRef/index.html)

[View Controller Catalog for iOS](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/PageViewControllers.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-05-03 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974