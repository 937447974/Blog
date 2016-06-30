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

##<a id="Configuring a View Controller Using Nib Files">1 Configuring a View Controller Using Nib Files

```swift
/// 通过xib初始化
public init(nibName nibNameOrNil: String?, bundle nibBundleOrNil: NSBundle?)
public init?(coder aDecoder: NSCoder)
/// nib的name
public var nibName: String? { get } // The name of the nib to be loaded to instantiate the view.
/// 资源包
public var nibBundle: NSBundle? { get } // The bundle from which to load the nib.
```

##<a id="Interacting with Storyboards and Segues">2 Interacting with Storyboards and Segues

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

##<a id="Managing the View">3 Managing the View

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

##<a id="Presenting View Controllers">4 Presenting View Controllers

```swift
```

##<a id="Supporting Custom Transitions and Presentations">5 Supporting Custom Transitions and Presentations

```swift
```

##<a id="Responding to View Events">6 Responding to View Events

```swift
```

##<a id="Configuring the View’s Layout Behavior">7 Configuring the View’s Layout Behavior

```swift
```

##<a id="Testing for Specific Kinds of View Transitions">8 Testing for Specific Kinds of View Transitions

```swift
```

##<a id="Configuring the View Rotation Settings">9 Configuring the View Rotation Settings

```swift
```

##<a id="Adapting to Environment Changes">10 Adapting to Environment Changes

```swift
```

##<a id="Managing Child View Controllers in a Custom Container">11 Managing Child View Controllers in a Custom Container

```swift
```

##<a id="Responding to Containment Events">12 Responding to Containment Events

```swift
```

##<a id="Getting Other Related View Controllers">13 Getting Other Related View Controllers

```swift
```

##<a id="Handling Memory Warnings">14 Handling Memory Warnings

```swift
```

##<a id="Managing State Restoration">15 Managing State Restoration

```swift
```

##<a id="Supporting App Extensions">16 Supporting App Extensions

```swift
```

##<a id="Working With 3D Touch Previews and Preview Quick Actions">17 Working With 3D Touch Previews and Preview Quick Actions

```swift
```

##<a id="Managing the Status Bar">18 Managing the Status Bar

```swift
```

##<a id="Configuring a Navigation Interface">19 Configuring a Navigation Interface

```swift
```

##<a id="Configuring Tab Bar Items">20 Configuring Tab Bar Items

```swift
```

##<a id="Adding Editing Behaviors to Your View Controller">21 Adding Editing Behaviors to Your View Controller

```swift
```

##<a id="Accessing the Available Key Commands">22 Accessing the Available Key Commands

```swift
```

##<a id="Managing Banner Ads">23 Managing Banner Ads

```swift
```

##<a id="Determining Whether the View Controller is Displaying an Ad">24 Determining Whether the View Controller is Displaying an Ad

```swift
```

##<a id="Managing Interstitial Ads">25 Managing Interstitial Ads

```swift
```

##<a id="Deprecated">26 Deprecated

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