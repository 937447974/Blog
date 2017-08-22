页面视图控制器让用户以翻书的形式滑动页面，每一页都有自己的UIViewController。我们可以使用自己的方式过渡动画。

相关类逻辑如图所示。

![](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Art/page_view.png)

# Creating Page View Controllers

```swift
// 初始化
public init(transitionStyle style: UIPageViewControllerTransitionStyle, navigationOrientation: UIPageViewControllerNavigationOrientation, options: [String : AnyObject]?)
// 导航代理
weak public var delegate: UIPageViewControllerDelegate?
// 数据源代理
weak public var dataSource: UIPageViewControllerDataSource?
```

# Providing Content

```swift
// 所有手势
public var gestureRecognizers: [UIGestureRecognizer] { get }
// 当前显示的UIViewController
public var viewControllers: [UIViewController]? { get }
// 替换UIViewController
public func setViewControllers(viewControllers: [UIViewController]?, direction: UIPageViewControllerNavigationDirection, animated: Bool, completion: ((Bool) -> Void)?)
```

# Display Options

```swift
// 翻页的样式
public var transitionStyle: UIPageViewControllerTransitionStyle { get }
// 分页的方向
public var navigationOrientation: UIPageViewControllerNavigationOrientation { get }
// 书脊位置，如果transitionStyle=PageCurl，spineLocation默认为Min
public var spineLocation: UIPageViewControllerSpineLocation { get }
// 是否多页面显示，如果spineLocation为Mid,doubleSide必须设置为true
public var doubleSided: Bool
```

&#160;

----------

# Appendix

## Related Documentation

[UIPageViewController Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIPageViewControllerClassReferenceClassRef/index.html)

[View Controller Catalog for iOS](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/PageViewControllers.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-05-03 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974