[UITabBarController](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UITabBarController.md)

[UITabBar](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UITabBar.md)

[UITabBarItem](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UITabBarItem.md)

----

UITabBarController是UITabBar的控制器，这是一个很强大的控制器，在各大app中经常能够看见。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016011002.jpg)

如上图所示的精品推荐、排行榜和探索等按钮就是一个UITabBar，它和UIToolBar很类似，都是处于屏幕的最下方，和UIToolBar不同的地方是它是系统级管理VC的，能够快速跳转到另一个分支的VC中。

#1 Accessing the Tab Bar Controller Properties

```swift
/// 代理，监听相关跳转信息
weak public var delegate: UITabBarControllerDelegate?
@available(iOS 3.0, *)
/// 获取UITabBar
public var tabBar: UITabBar { get }
```

#2 Managing the View Controllers

```swift
/// 当前管理的所有UIViewController
public var viewControllers: [UIViewController]?

/// 是否动画设置[UIViewController]
///
/// - parameter viewControllers : [UIViewController]
/// - parameter animated: Bool
///
/// - returns: void
public func setViewControllers(viewControllers: [UIViewController]?, animated: Bool)

/// 更多的导航界面
public var moreNavigationController: UINavigationController { get }
/// 更多中显示的[UIViewController]
public var customizableViewControllers: [UIViewController]?
```

#3 Managing the Selected Tab

```swift
/// 选中的UIViewController
unowned(unsafe) public var selectedViewController: UIViewController?
/// 选中的UIViewController序号，从左到右[0,max]
public var selectedIndex: Int
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UIKit Framework Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

[UITabBarController Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITabBarController_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-10 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog