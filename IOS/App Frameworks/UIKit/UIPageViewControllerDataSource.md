UIPageViewControllerDataSource是UIPageViewController的数据源代理。

# Providing View Controllers


```swift
// 上一个页面
@available(iOS 5.0, *)
public func pageViewController(pageViewController: UIPageViewController, viewControllerBeforeViewController viewController: UIViewController) -> UIViewController?

// 下一个页面
@available(iOS 5.0, *)
public func pageViewController(pageViewController: UIPageViewController, viewControllerAfterViewController viewController: UIViewController) -> UIViewController?
```

# Supporting a Page Indicator

```swift
// 要显示的总UIViewController
@available(iOS 6.0, *)
optional public func presentationCountForPageViewController(pageViewController: UIPageViewController) -> Int

// UIViewController显示的索引
@available(iOS 6.0, *)
optional public func presentationIndexForPageViewController(pageViewController: UIPageViewController) -> Int
```

&#160;

----------

# Appendix

## Related Documentation

[UIPageViewControllerDataSource Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIPageViewControllerDataSourceProtocolRef/index.html)

[View Controller Catalog for iOS](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/PageViewControllers.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-05-03 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974