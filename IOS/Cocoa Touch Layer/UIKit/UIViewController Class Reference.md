UIViewController 是控制器的基类。生命周期如图所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016062001.png)

##1 Configuring a View Controller Using Nib Files

```swift
/// 通过xib初始化
public init(nibName nibNameOrNil: String?, bundle nibBundleOrNil: NSBundle?)
public init?(coder aDecoder: NSCoder)
/// nib的name
public var nibName: String? { get } // The name of the nib to be loaded to instantiate the view.
/// 资源包
public var nibBundle: NSBundle? { get } // The bundle from which to load the nib.
```

##2 Interacting with Storyboards and Segues

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

##3 Managing the View

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

##4 Presenting View Controllers

```swift
/// 专场动画的风格
@available(iOS 3.0, *)
public var modalTransitionStyle: UIModalTransitionStyle
/// 控制器展示风格
@available(iOS 3.2, *)
public var modalPresentationStyle: UIModalPresentationStyle
/// 是否弹窗模式展示
@available(iOS 3.2, *)
public var modalInPopover: Bool
/// 显示一个UIViewController
@available(iOS 8.0, *)
public func showViewController(vc: UIViewController, sender: AnyObject?)    
/// 显示detail视图
@available(iOS 8.0, *)
public func showDetailViewController(vc: UIViewController, sender: AnyObject?)
/// modally方式显示UIViewController
@available(iOS 5.0, *)
public func presentViewController(viewControllerToPresent: UIViewController, animated flag: Bool, completion: (() -> Void)?)
// 隐藏当前UIViewController
@available(iOS 5.0, *)
public func dismissViewControllerAnimated(flag: Bool, completion: (() -> Void)?)
// 当前控制器后是否有控制器
@available(iOS 5.0, *)
public var definesPresentationContext: Bool
// 是否使用视图控制器的过过渡样式
@available(iOS 5.0, *)
public var providesPresentationContextTransitionStyle: Bool
/// 是否控制键盘关闭
@available(iOS 4.3, *)
public func disablesAutomaticKeyboardDismissal() -> Bool
```

##5 Supporting Custom Transitions and Presentations

```swift
// 转场的代理
@available(iOS 7.0, *)
weak public var transitioningDelegate: UIViewControllerTransitioningDelegate?
// 转场对象
@available(iOS 7.0, *)
public func transitionCoordinator() -> UIViewControllerTransitionCoordinator?
// 获取响应事件的对象
@available(iOS 8.0, *)
public func targetViewControllerForAction(action: Selector, sender: AnyObject?) -> UIViewController?
// 最近的上一个UIViewController
@available(iOS 8.0, *)
public var presentationController: UIPresentationController? { get }
// 最近关闭的控制器
@available(iOS 8.0, *)
public var popoverPresentationController: UIPopoverPresentationController? { get }
```

##6 Responding to View Events

```swift
// 视图将要显示
public func viewWillAppear(animated: Bool)
// 视图已显示
public func viewDidAppear(animated: Bool) 
// 视图将要消失
public func viewWillDisappear(animated: Bool) 
// 视图已消失
public func viewDidDisappear(animated: Bool)
```

##7 Configuring the View’s Layout Behavior

```swift
// 布局将要发生改变
@available(iOS 5.0, *)
public func viewWillLayoutSubviews()
// 布局已发生改变
@available(iOS 5.0, *)
public func viewDidLayoutSubviews()
// 更新约束
@available(iOS 6.0, *)
public func updateViewConstraints()
// 顶部约束
@available(iOS 7.0, *)
public var topLayoutGuide: UILayoutSupport { get }
// 底部约束
@available(iOS 7.0, *)
public var bottomLayoutGuide: UILayoutSupport { get }
// 边缘约束
@available(iOS 7.0, *)
public var edgesForExtendedLayout: UIRectEdge
// 是否延长布局包含透明的bar
@available(iOS 7.0, *)
public var extendedLayoutIncludesOpaqueBars: Bool
// 是否自动调整ScrollView的Inset
@available(iOS 7.0, *)
public var automaticallyAdjustsScrollViewInsets: Bool
```

##8 Testing for Specific Kinds of View Transitions

```swift
// 是否从父视图移除
@available(iOS 5.0, *)
public func isMovingToParentViewController() -> Bool
// 是否移入父视图
@available(iOS 5.0, *)
public func isMovingFromParentViewController() -> Bool
// 正在显示
@available(iOS 5.0, *)
public func isBeingPresented() -> Bool
// 正在隐藏
@available(iOS 5.0, *)
public func isBeingDismissed() -> Bool
```

##9 Configuring the View Rotation Settings

```swift
// 内容是否自动旋转
@available(iOS 6.0, *)
public func shouldAutorotate() -> Bool
// 支持的旋转方向
@available(iOS 6.0, *)
public func supportedInterfaceOrientations() -> UIInterfaceOrientationMask
// 界面显示时呈现的方向
@available(iOS 6.0, *)
public func preferredInterfaceOrientationForPresentation() -> UIInterfaceOrientation
// 尝试旋转方向
@available(iOS 5.0, *)
public class func attemptRotationToDeviceOrientation()
```

##10 Adapting to Environment Changes

```swift
// 显示副视图
@available(iOS 8.0, *)
public func collapseSecondaryViewController(secondaryViewController: UIViewController, forSplitViewController splitViewController: UISplitViewController)
// 隐藏副视图
@available(iOS 8.0, *)
public func separateSecondaryViewControllerForSplitViewController(splitViewController: UISplitViewController) -> UIViewController?
```

##11 Managing Child View Controllers in a Custom Container

```swift
// 子控制器
@available(iOS 5.0, *)
public var childViewControllers: [UIViewController] { get }
// 添加子控制器
@available(iOS 5.0, *)
public func addChildViewController(childController: UIViewController)
// 从父控制器移除当前控制器
@available(iOS 5.0, *)
public func removeFromParentViewController()
// 控制器转场
@available(iOS 5.0, *)
public func transitionFromViewController(fromViewController: UIViewController, toViewController: UIViewController, duration: NSTimeInterval, options: UIViewAnimationOptions, animations: (() -> Void)?, completion: ((Bool) -> Void)?)
// 是否自动向子控制器发生view变化
@available(iOS 6.0, *)
public func shouldAutomaticallyForwardAppearanceMethods() -> Bool
// 开始显示变化
@available(iOS 5.0, *)
public func beginAppearanceTransition(isAppearing: Bool, animated: Bool)
// 结束显示变化
@available(iOS 5.0, *)
public func endAppearanceTransition()
// 设置子控制器的特征变化
@available(iOS 8.0, *)
public func setOverrideTraitCollection(collection: UITraitCollection?, forChildViewController childViewController: UIViewController)
// 获取子控制器的特征变化
@available(iOS 8.0, *)
public func overrideTraitCollectionForChildViewController(childViewController: UIViewController) -> UITraitCollection?
```

##12 Responding to Containment Events

```swift
// 将要移动到父VC
@available(iOS 5.0, *)
public func willMoveToParentViewController(parent: UIViewController?)
// 已经移动到父VC
@available(iOS 5.0, *)
public func didMoveToParentViewController(parent: UIViewController?)
```

##13 Getting Other Related View Controllers

```swift
// 父控制器
weak public var parentViewController: UIViewController? { get }
// presented当前VC的VC,或其最近的父视图
@available(iOS 5.0, *)
public var presentedViewController: UIViewController? { get }
// presented当前VC的VC,或其最远的父视图
@available(iOS 5.0, *)
public var presentingViewController: UIViewController? { get }
// 导航控制器
public var navigationController: UINavigationController? { get }
// split控制器
public var splitViewController: UISplitViewController? { get }
// tabBar控制器
public var tabBarController: UITabBarController? { get }
```

##14 Handling Memory Warnings

```swift
// 内存警告
public func didReceiveMemoryWarning()
```

##15 Managing State Restoration

```swift
// 状态标示符
@available(iOS 6.0, *)
public var restorationIdentifier: String?
// 恢复状态的类
@available(iOS 6.0, *)
public var restorationClass: AnyObject.Type?
// 编码
@available(iOS 6.0, *)
public func encodeRestorableStateWithCoder(coder: NSCoder)
// 解码
@available(iOS 6.0, *)
public func decodeRestorableStateWithCoder(coder: NSCoder)
// 完成恢复
@available(iOS 7.0, *)
public func applicationFinishedRestoringState()
```

##16 Supporting App Extensions

```swift
// request的上下文
@available(iOS 8.0, *)
public var extensionContext: NSExtensionContext? { get }
```

##17 Working With 3D Touch Previews and Preview Quick Actions

```swift
// 注册3D Touch的显示view
@available(iOS 9.0, *)
public func registerForPreviewingWithDelegate(delegate: UIViewControllerPreviewingDelegate, sourceView: UIView) -> UIViewControllerPreviewing
// 移除3D Touch
@available(iOS 9.0, *)
public func unregisterForPreviewingWithContext(previewing: UIViewControllerPreviewing)
// 相关按钮
@available(iOS 9.0, *)
public func previewActionItems() -> [UIPreviewActionItem]
```

##18 Managing the Status Bar

```swift
// 状态栏样式控制器
@available(iOS 7.0, *)
public func childViewControllerForStatusBarStyle() -> UIViewController?
// 状态栏隐藏控制器
@available(iOS 7.0, *)
public func childViewControllerForStatusBarHidden() -> UIViewController?
// 状态栏的样式
@available(iOS 7.0, *)
public func preferredStatusBarStyle() -> UIStatusBarStyle
// 状态栏是否隐藏
@available(iOS 7.0, *)
public func prefersStatusBarHidden() -> Bool
// 是否管理状态栏
@available(iOS 7.0, *)
public var modalPresentationCapturesStatusBarAppearance: Bool
// 状态栏变化效果
@available(iOS 7.0, *)
public func preferredStatusBarUpdateAnimation() -> UIStatusBarAnimation
// 更新状态栏
@available(iOS 7.0, *)
public func setNeedsStatusBarAppearanceUpdate()
```

##19 Configuring a Navigation Interface

```swift
// 导航按钮
public var navigationItem: UINavigationItem { get } 
// push情况下是否隐藏bar
public var hidesBottomBarWhenPushed: Bool
// toolbar的按钮
@available(iOS 3.0, *)
public var toolbarItems: [UIBarButtonItem]?
// 动画设置toolbar的按钮
@available(iOS 3.0, *)
public func setToolbarItems(toolbarItems: [UIBarButtonItem]?, animated: Bool)
```

##20 Configuring Tab Bar Items

```swift
// tabbar的按钮
public var tabBarItem: UITabBarItem!
```

##21 Adding Editing Behaviors to Your View Controller

```swift
// 是否编辑模式
public var editing: Bool
// 动画设置编辑模式
public func setEditing(editing: Bool, animated: Bool) 
// 编辑模式下的按钮    
public func editButtonItem() -> UIBarButtonItem
```

##22 Accessing the Available Key Commands

```swift
// 添加UIKeyCommand
@available(iOS 9.0, *)
public func addKeyCommand(keyCommand: UIKeyCommand)
// 移除UIKeyCommand
@available(iOS 9.0, *)
public func removeKeyCommand(keyCommand: UIKeyCommand)
```

##23 Managing Banner Ads

```swift
// 能否显示广告位
@available(iOS 7.0, *)
public var canDisplayBannerAds: Bool
// 原始View
@available(iOS 7.0, *)
public var originalContentView: UIView! { get }
```

##24 Determining Whether the View Controller is Displaying an Ad

```swift
// 是否满屏幕显示
@available(iOS 7.0, *)
public var presentingFullScreenAd: Bool { get }
// 是否显示广告位
@available(iOS 7.0, *)
public var displayingBannerAd: Bool { get }
```

##25 Managing Interstitial Ads

```swift
// 准备显示广告资产
@available(iOS 7.0, *)
public class func prepareInterstitialAds()
// 广告展示策略
@available(iOS 7.0, *)
public var interstitialPresentationPolicy: ADInterstitialPresentationPolicy
// 要求框架显示一个广告
@available(iOS 7.0, *)
public func requestInterstitialAdPresentation() -> Bool
// 广告将要显示
@available(iOS 7.0, *)
public var shouldPresentInterstitialAd: Bool { get }
```

----------

#Appendix

##Related Documentation

[UIViewController Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIViewController_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-20 | 博文创建 |
| 2016-07-21 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974