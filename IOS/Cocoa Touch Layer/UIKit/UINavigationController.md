1. [简介](#简介)
    1. [导航效果图](#导航效果图)
    2. [导航控制器的对象管理](#导航控制器的对象管理)
    3. [导航控制器内的视图](#导航控制器内的视图)
2. [Creating Navigation Controllers](#Creating_Navigation_Controllers)
3. [Accessing Items on the Navigation Stack](#Accessing_Items_on_the_Navigation_Stack)
4. [Pushing and Popping Stack Items](#Pushing_and_Popping_Stack_Items)
5. [Configuring Navigation Bars](#Configuring_Navigation_Bars)
6. [Configuring Custom Toolbars](#Configuring_Custom_Toolbars)
7. [Hiding the Navigation Bar](#Hiding_the_Navigation_Bar)
8. [Accessing the Delegate](#Accessing_the_Delegate)
9. [Action Method for Displaying View Controllers](#Action_Method_for_Displaying_View_Controllers)

---

#<a id="简介"/>1 简介

在开发app的过程中任何一个项目都会用到UINavigationController，这是一个很有用的控件，它主要控制界面的跳转和导航。

##<a id="导航效果图"/>1.1 导航效果图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016010802.png)

##<a id="导航控制器的对象管理"/>1.2 导航控制器的对象管理

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016010801.jpg)

##<a id="导航控制器内的视图"/>1.3 导航控制器内的视图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016010803.png)


#<a id="Creating_Navigation_Controllers"/>2 Creating Navigation Controllers

```swift
/// 使用自定义的navigationBar和toolbar
///
/// - parameter navigationBarClass : AnyClass
/// - parameter toolbarClass : AnyClass
///
/// - returns: UINavigationController
public init(navigationBarClass: AnyClass?, toolbarClass: AnyClass?)
    
    
/// 初始化UINavigationController并指定根VC
///
/// - parameter rootViewController : UIViewController
///
/// - returns: UINavigationController
public init(rootViewController: UIViewController)
```

#<a id="Accessing_Items_on_the_Navigation_Stack"/>3 Accessing Items on the Navigation Stack

```swift
/// 顶部UIViewController
public var topViewController: UIViewController? { get } // The top view controller on the stack.
/// 当前UIViewController
public var visibleViewController: UIViewController? { get } // Return modal view controller if it exists. Otherwise the top view controller.
    
/// 当前UINavigationController管理的UIViewController
public var viewControllers: [UIViewController] // The current view controller stack.
    
/// 替换堆栈控件的[UIViewController]
///
/// - parameter viewControllers : 要替换的视图
/// - parameter animated : 是否动画替换
///
/// - returns: void
@available(iOS 3.0, *)
public func setViewControllers(viewControllers: [UIViewController], animated: Bool)
```

#<a id="Pushing_and_Popping_Stack_Items"/>4 Pushing and Popping Stack Items

```swift
/// push进入下一个VC
///
/// - parameter viewController : UIViewController 目标VC
/// - parameter animated : Bool 是否动画跳转
///
/// - returns: void
public func pushViewController(viewController: UIViewController, animated: Bool)
    
/// pop回到上一个VC
///
/// - parameter animated : Bool 是否动画跳转
///
/// - returns: 当前UIViewController
public func popViewControllerAnimated(animated: Bool) -> UIViewController?
    
/// pop回到指定VC
///
/// - parameter viewController : UIViewController 目标VC
/// - parameter animated : Bool 是否动画跳转
///
/// - returns: 弹出的[UIViewController]
public func popToViewController(viewController: UIViewController, animated: Bool) -> [UIViewController]?
    
/// pop回到第一个VC
///
/// - parameter animated : Bool 是否动画跳转
///
/// - returns: 弹出的[UIViewController]
public func popToRootViewControllerAnimated(animated: Bool) -> [UIViewController]?
    
/// 手势滑动返回的手势
public var interactivePopGestureRecognizer: UIGestureRecognizer? { get }
```

#<a id="Configuring_Navigation_Bars"/>5 Configuring Navigation Bars

```swift
/// 获取UINavigationBar
public var navigationBar: UINavigationBar { get }

/// 是否隐藏UINavigationBar
public var navigationBarHidden: Bool
    
/// 是否动画隐藏UINavigationBar
///
/// - parameter hidden : 是否隐藏
/// - parameter animated : 是否动画
///
/// - returns: void
public func setNavigationBarHidden(hidden: Bool, animated: Bool)
```

#<a id="Configuring_Custom_Toolbars"/>6 Configuring Custom Toolbars

```swift
/// 获取UIToolbar
@available(iOS 3.0, *)
public var toolbar: UIToolbar! { get }
/// 是否隐藏UIToolbar
@available(iOS 3.0, *)
public var toolbarHidden: Bool // Defaults to YES, i.e. hidden.
    
/// 是否动画隐藏UIToolbar
///
/// - parameter hidden : 是否隐藏
/// - parameter animated : 是否动画
///
/// - returns: void
@available(iOS 3.0, *)
public func setToolbarHidden(hidden: Bool, animated: Bool)
```

#<a id="Hiding_the_Navigation_Bar"/>7 Hiding the Navigation Bar

```swift
/// 是否点击屏幕隐藏UINavigationBar
@available(iOS 8.0, *)
public var hidesBarsOnTap: Bool
/// 点击屏幕隐藏UINavigationBar的手势
@available(iOS 8.0, *)
unowned(unsafe) public var barHideOnTapGestureRecognizer: UITapGestureRecognizer { get }
    
/// 是否点击上下滑动隐藏UINavigationBar
@available(iOS 8.0, *)
public var hidesBarsOnSwipe: Bool
/// 滑动隐藏UINavigationBar的手势
@available(iOS 8.0, *)
public var barHideOnSwipeGestureRecognizer: UIPanGestureRecognizer { get }
    
/// 是否键盘打开时隐藏UINavigationBar
@available(iOS 8.0, *)
public var hidesBarsWhenKeyboardAppears: Bool

 /// 是否横屏隐藏UINavigationBar
@available(iOS 8.0, *)
public var hidesBarsWhenVerticallyCompact: Bool
```

#<a id="Accessing_the_Delegate"/>8 Accessing the Delegate

```swift
/// 跳转监听
weak var delegate: UINavigationControllerDelegate?
```


#<a id="ActionMethodforDisplayingViewControllers"/>9 Action Method for Displaying View Controllers

```swift
/// 和pushViewController:animated:相同的功能，可携带参数传递
///
/// - parameter vc : UIViewController
/// - parameter sender : 携带参数
///
/// - returns: void
func showViewController(_ vc: UIViewController, sender sender: AnyObject?)
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UIKit Framework Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

[UINavigationController Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UINavigationController_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-09 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog