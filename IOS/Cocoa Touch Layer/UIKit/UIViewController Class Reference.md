1. [Configuring a View Controller Using Nib Files](#Configuring a View Controller Using Nib Files)
2. [Interacting with Storyboards and Segues](#Interacting with Storyboards and Segues)
3. [Managing the View](#Managing the View)
4. [Presenting View Controllers](#Presenting View Controllers)
5. [Supporting Custom Transitions and Presentations](#Supporting Custom Transitions and Presentations)
6. [Responding to View Events](#Responding to View Events)
7. [Configuring the View’s Layout Behavior](#Configuring the View’s Layout Behavior)
8. [Testing for Specific Kinds of View Transitions](#Testing for Specific Kinds of View Transitions)
9. [Configuring the View Rotation Settings](#Configuring the View Rotation Settings)
10. [Adapting to Environment Changes](#Adapting to Environment Changes)
11. [Managing Child View Controllers in a Custom Container](#Managing Child View Controllers in a Custom Container)
12. [Responding to Containment Events](#Responding to Containment Events)
13. [Getting Other Related View Controllers](#Getting Other Related View Controllers)
14. [Handling Memory Warnings](#Handling Memory Warnings)
15. [Managing State Restoration](#Managing State Restoration)
16. [Supporting App Extensions](#Supporting App Extensions)
17. [Working With 3D Touch Previews and Preview Quick Actions](#Working With 3D Touch Previews and Preview Quick Actions)
18. [Managing the Status Bar](#Managing the Status Bar)
19. [Configuring a Navigation Interface](#Configuring a Navigation Interface)
20. [Configuring Tab Bar Items](#Configuring Tab Bar Items)
21. [Adding Editing Behaviors to Your View Controller](#Adding Editing Behaviors to Your View Controller)
22. [Accessing the Available Key Commands](#Accessing the Available Key Commands)
23. [Managing Banner Ads](#Managing Banner Ads)
24. [Determining Whether the View Controller is Displaying an Ad](#Determining Whether the View Controller is Displaying an Ad)
25. [Managing Interstitial Ads](#Managing Interstitial Ads)
26. [Deprecated](#Deprecated)

----

UIViewController 是控制器的基类。生命周期如图所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016062001.png)

##<a id="Configuring a View Controller Using Nib Files">1 Initializing a View Object

```swift
/// 通过xib初始化
public init(nibName nibNameOrNil: String?, bundle nibBundleOrNil: NSBundle?)
public init?(coder aDecoder: NSCoder)
/// nib的name
public var nibName: String? { get } // The name of the nib to be loaded to instantiate the view.
/// 资源包
public var nibBundle: NSBundle? { get } // The bundle from which to load the nib.
```

##<a id="Configuring a View Controller Using Nib Files">2 Configuring a View Controller Using Nib Files

```swift
/// 所在的storyboard
@available(iOS 5.0, *)
public var storyboard: UIStoryboard? { get }

/// 能否跳转页面
@available(iOS 6.0, *)
public func shouldPerformSegueWithIdentifier(identifier: String, sender: AnyObject?) -> Bool

/// 根据路径跳转页面
@available(iOS 5.0, *)
public func performSegueWithIdentifier(identifier: String, sender: AnyObject?)
    
/// 跳转到UIStoryboard
@available(iOS 5.0, *)
public func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?)

/// 获得其所有子视图控制器数组用来搜索Unwind Segue所在位置控制器
@available(iOS 9.0, *)
public func allowedChildViewControllersForUnwindingFromSource(source: UIStoryboardUnwindSegueSource) -> [UIViewController]

/// 返回管理的目标控制器对象
@available(iOS 9.0, *)
public func childViewControllerContainingSegueSource(source: UIStoryboardUnwindSegueSource) -> UIViewController?
    
/// 判断能否执行此action
@available(iOS 6.0, *)
public func canPerformUnwindSegueAction(action: Selector, fromViewController: UIViewController, withSender sender: AnyObject) -> Bool
    
/// 过度到一个新的视图控制器
@available(iOS 9.0, *)
public func unwindForSegue(unwindSegue: UIStoryboardSegue, towardsViewController subsequentVC: UIViewController)
```

##<a id="Interacting with Storyboards and Segues">3 Interacting with Storyboards and Segues

```swift
/// 显示的view
public var view: UIView!
/// 正在加载到内存中
public func loadView()
/// 视图未加载时，加载视图
@available(iOS 9.0, *)
public func loadViewIfNeeded()
/// 视图load后返回其view
@available(iOS 9.0, *)
public var viewIfLoaded: UIView? { get }
/// load结束
public func viewDidLoad()
/// 是否正在load
@available(iOS 3.0, *)
public func isViewLoaded() -> Bool
/// 标题
public var title: String?
/// 视图大小
@available(iOS 7.0, *)
public var preferredContentSize: CGSize
```

##<a id="">4 Managing the View

```swift
```

##<a id="">5 Presenting View Controllers

```swift
```

##<a id="">6 Supporting Custom Transitions and Presentations

```swift
```

##<a id="">7 Responding to View Events

```swift
```

##<a id="">8 Configuring the View’s Layout Behavior

```swift
```

##<a id="">9 Testing for Specific Kinds of View Transitions

```swift
```

##<a id="">10 Configuring the View Rotation Settings

```swift
```

##<a id="">11 Adapting to Environment Changes

```swift
```

##<a id="">12 Managing Child View Controllers in a Custom Container

```swift
```

##<a id="">13 Responding to Containment Events

```swift
```

##<a id="">14 Getting Other Related View Controllers

```swift
```

##<a id="">15 Handling Memory Warnings

```swift
```

##<a id="">16 Managing State Restoration

```swift
```

##<a id="">17 Supporting App Extensions

```swift
```

##<a id="">18 Working With 3D Touch Previews and Preview Quick Actions

```swift
```

##<a id="">19 Managing the Status Bar

```swift
```

##<a id="">20 Configuring a Navigation Interface

```swift
```

##<a id="">21 Configuring Tab Bar Items

```swift
```

##<a id="">22 Adding Editing Behaviors to Your View Controller

```swift
```

##<a id="">23 Accessing the Available Key Commands

```swift
```

##<a id="">24 Managing Banner Ads

```swift
```

##<a id="">25 Determining Whether the View Controller is Displaying an Ad

```swift
```

##<a id="">26 Managing Interstitial Ads

```swift
```

##<a id="">27 Deprecated

```swift
```


----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation


##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-20 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974