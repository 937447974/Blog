1. [Initializing a View Object](#Initializing a View Object)
2. [Configuring a View’s Visual Appearance](#Configuring a View’s Visual Appearance)
3. [Configuring the Event-Related Behavior](#Configuring the Event-Related Behavior)
4. [Configuring the Bounds and Frame Rectangles](#Configuring the Bounds and Frame Rectangles)
5. [Managing the View Hierarchy](#Managing the View Hierarchy)
6. [Configuring the Resizing Behavior](#Configuring the Resizing Behavior)
7. [Laying out Subviews](#Laying out Subviews)
8. [Creating Constraints Using Layout Anchors](#Creating Constraints Using Layout Anchors)
9. [Managing the View’s Constraints](#Managing the View’s Constraints)
10. [Working with Layout Guides](#Working with Layout Guides)
11. [Measuring in Auto Layout](#Measuring in Auto Layout)
12. [Aligning Views in Auto Layout](#Aligning Views in Auto Layout)
13. [Triggering Auto Layout](#Triggering Auto Layout)
14. [Debugging Auto Layout](#Debugging Auto Layout)
15. [Managing the User Interface Direction](#Managing the User Interface Direction)
16. [Configuring Content Margins](#Configuring Content Margins)
17. [Drawing and Updating the View](#Drawing and Updating the View)
18. [Formatting Printed View Content](#Formatting Printed View Content)
19. [Managing Gesture Recognizers](#Managing Gesture Recognizers)
20. [Animating Views with Block Objects](#Animating Views with Block Objects)
21. [Animating Views](#Animating Views)
22. [Using Motion Effects](#Using Motion Effects)
23. [Preserving and Restoring State](#Preserving and Restoring State)
24. [Capturing a View Snapshot](#Capturing a View Snapshot)
25. [Identifying the View at Runtime](#Identifying the View at Runtime)
26. [Converting Between View Coordinate Systems](#Converting Between View Coordinate Systems)
27. [Hit Testing in a View](#Hit Testing in a View)
28. [Ending a View Editing Session](#Ending a View Editing Session)
29. [Observing View-Related Changes](#Observing View-Related Changes)
30. [Observing Focus](#Observing Focus)

----

# UIView

UIView是所有界面View的基类。

##<a id="Initializing a View Object">1 Initializing a View Object

```swift
/// 初始化
public init(frame: CGRect)
```

##<a id="Configuring a View’s Visual Appearance">2 Configuring a View’s Visual Appearance

```swift
@NSCopying public var backgroundColor: UIColor?
/// 是否隐藏，默认NO
 public var hidden: Bool
/// 透明度，默认1.0
public var alpha: CGFloat
/// 是否透明，默认YES
public var opaque: Bool
/// 默认色值 会随视图响应链传递
@available(iOS 7.0, *)
public var tintColor: UIColor!
/// tintColor传递策略
@available(iOS 7.0, *)
public var tintAdjustmentMode: UIViewTintAdjustmentMode
/// 父视图tintColor改变时，调用子类此方法
@available(iOS 7.0, *)
public func tintColorDidChange()
/// 确定子视图的显示范围在父视图内
public var clipsToBounds: Bool
/// 在drawRect View的时候，是否自动清除上一次的画线，默认yes
public var clearsContextBeforeDrawing: Bool
/// 透明层 上层透明时，底层显示
@available(iOS 8.0, *)
public var maskView: UIView?
/// layer层的class
public class func layerClass() -> AnyClass // default is [CALayer class].
/// 核心动画 layer层
public var layer: CALayer { get }
```

##<a id="Configuring the Event-Related Behavior">3 Configuring the Event-Related Behavior

```swift
/// 用户是否可交互
public var userInteractionEnabled: Bool // default is YES
/// 是否多手指交互
public var multipleTouchEnabled: Bool // default is NO
/// 是否拦截交互限制在当前层
public var exclusiveTouch: Bool // default is NO
```

##<a id="Configuring the Bounds and Frame Rectangles">4 Configuring the Bounds and Frame Rectangles

```swift
/// 显示区域
public var frame: CGRect
/// 子view可使用区域（zero origin, frame size）
public var bounds: CGRect
/// 中心点
public var center: CGPoint
/// 旋转、缩放等设置
public var transform: CGAffineTransform
```

##<a id="Managing the View Hierarchy">5 Managing the View Hierarchy

```swift
/// 父View
public var superview: UIView? { get }
/// 子view
public var subviews: [UIView] { get }
/// 所在window层
public var window: UIWindow? { get }

/// 添加子view
public func addSubview(view: UIView)    
/// 设置在某个子view在最上层
public func bringSubviewToFront(view: UIView)
/// 设置在某个子view在最下层
public func sendSubviewToBack(view: UIView)
    
/// 从父view中移除
public func removeFromSuperview()    
    
/// 添加子view在某个位置
public func insertSubview(view: UIView, atIndex index: Int)
/// 添加子view在某个view的上面
public func insertSubview(view: UIView, belowSubview siblingSubview: UIView)
/// 添加子view在某个view的下面
public func insertSubview(view: UIView, aboveSubview siblingSubview: UIView)
    
/// 交换两个view的位置
public func exchangeSubviewAtIndex(index1: Int, withSubviewAtIndex index2: Int)
/// 某个view是否是其子view
public func isDescendantOfView(view: UIView) -> Bool
```

##<a id="Configuring the Resizing Behavior">6 Configuring the Resizing Behavior

```swift
// 父view的size变化时，当前view的变化策略
public var autoresizingMask: UIViewAutoresizing
// 子view是否随当前view的size变化而变化
public var autoresizesSubviews: Bool
/// 内容变化范围
public var contentMode: UIViewContentMode // default is UIViewContentModeScaleToFill
/// 根据给定的size返回最适合显示的size
public func sizeThatFits(size: CGSize) -> CGSize
/// 通知当前view改变size，涉及子view
public func sizeToFit()

```

##<a id="Laying out Subviews">7 Laying out Subviews

```swift
/// 通知适当的时候改变layout
public func setNeedsLayout()
/// 如果有layout变化，立即改变layout
public func layoutIfNeeded()
/// 父view改变layout，当前view跟随变化，更精确的控制
public func layoutSubviews()
/// 是否允许AutoresizingMask转换成Autolayout
@available(iOS 6.0, *)
public var translatesAutoresizingMaskIntoConstraints: Bool
/// 是否支持约束布局
@available(iOS 6.0, *)
public class func requiresConstraintBasedLayout() -> Bool
```

##<a id="Creating Constraints Using Layout Anchors">8 Creating Constraints Using Layout Anchors

```swift
/* Constraint creation conveniences. See NSLayoutAnchor.h for details. */
@available(iOS 9.0, *)
public var leadingAnchor: NSLayoutXAxisAnchor { get }
@available(iOS 9.0, *)
public var trailingAnchor: NSLayoutXAxisAnchor { get }
@available(iOS 9.0, *)
public var leftAnchor: NSLayoutXAxisAnchor { get }
@available(iOS 9.0, *)
public var rightAnchor: NSLayoutXAxisAnchor { get }
@available(iOS 9.0, *)
public var topAnchor: NSLayoutYAxisAnchor { get }
@available(iOS 9.0, *)
public var bottomAnchor: NSLayoutYAxisAnchor { get }
@available(iOS 9.0, *)
public var widthAnchor: NSLayoutDimension { get }
@available(iOS 9.0, *)
public var heightAnchor: NSLayoutDimension { get }
@available(iOS 9.0, *)
public var centerXAnchor: NSLayoutXAxisAnchor { get }
@available(iOS 9.0, *)
public var centerYAnchor: NSLayoutYAxisAnchor { get }
@available(iOS 9.0, *)
public var firstBaselineAnchor: NSLayoutYAxisAnchor { get }
@available(iOS 9.0, *)
public var lastBaselineAnchor: NSLayoutYAxisAnchor { get }
```

##<a id="Managing the View’s Constraints">9 Managing the View’s Constraints

```swift
/// 当前约束集合
@available(iOS 6.0, *)
public var constraints: [NSLayoutConstraint] { get }
/// 添加约束
@available(iOS 6.0, *)
public func addConstraint(constraint: NSLayoutConstraint)
/// 添加多个约束
@available(iOS 6.0, *)
public func addConstraints(constraints: [NSLayoutConstraint])
/// 移除约束
@available(iOS 6.0, *)
public func removeConstraint(constraint: NSLayoutConstraint)
/// 移除多个约束
@available(iOS 6.0, *)
public func removeConstraints(constraints: [NSLayoutConstraint])
```

##<a id="Working with Layout Guides">10 Working with Layout Guides

```swift
/// 约束布局
@available(iOS 9.0, *)
public var layoutGuides: [UILayoutGuide] { get }
/// 添加约束布局
@available(iOS 9.0, *)
public func addLayoutGuide(layoutGuide: UILayoutGuide)
/// 移除约束布局
@available(iOS 9.0, *)
public func removeLayoutGuide(layoutGuide: UILayoutGuide)
/// 有Margins的约束
@available(iOS 9.0, *)
public var layoutMarginsGuide: UILayoutGuide { get }    
/// 宽度约束
@available(iOS 9.0, *)
public var readableContentGuide: UILayoutGuide { get }
```

##<a id="Measuring in Auto Layout">11 Measuring in Auto Layout

```swift
/// 返回满足约束的size
@available(iOS 6.0, *)
public func systemLayoutSizeFittingSize(targetSize: CGSize) -> CGSize
/// 返回满足约束的size
@available(iOS 8.0, *)
public func systemLayoutSizeFittingSize(targetSize: CGSize, withHorizontalFittingPriority horizontalFittingPriority: UILayoutPriority, verticalFittingPriority: UILayoutPriority) -> CGSize
/// 返回显示所需size
@available(iOS 6.0, *)
public func intrinsicContentSize() -> CGSize
/// 使固有的size作废
@available(iOS 6.0, *)
public func invalidateIntrinsicContentSize()
/// 获取缩小的优先级
@available(iOS 6.0, *)
public func contentCompressionResistancePriorityForAxis(axis: UILayoutConstraintAxis) -> UILayoutPriority
/// 设置缩小的优先级
@available(iOS 6.0, *)
public func setContentCompressionResistancePriority(priority: UILayoutPriority, forAxis axis: UILayoutConstraintAxis)
/// 获取拉伸的优先级
@available(iOS 6.0, *)
public func contentHuggingPriorityForAxis(axis: UILayoutConstraintAxis) -> UILayoutPriority
/// 设置拉伸的优先级
@available(iOS 6.0, *)
public func setContentHuggingPriority(priority: UILayoutPriority, forAxis axis: UILayoutConstraintAxis)
```

##<a id="Aligning Views in Auto Layout">12 Aligning Views in Auto Layout

```swift
/// 返回给定框架的视图的对齐矩阵
@available(iOS 6.0, *)
public func alignmentRectForFrame(frame: CGRect) -> CGRect
/// 返回给定对齐矩形的视图的frame
@available(iOS 6.0, *)
public func frameForAlignmentRect(alignmentRect: CGRect) -> CGRect
/// 返回从视图的frame上定义的对齐矩阵的边框
@available(iOS 6.0, *)
public func alignmentRectInsets() -> UIEdgeInsets
    
 /// 返回满足上基线约束条件的视图
@available(iOS, introduced=6.0, deprecated=9.0, message="Override -viewForFirstBaselineLayout or -viewForLastBaselineLayout as appropriate, instead")
public func viewForBaselineLayout() -> UIView    
/// 返回满足上基线约束条件的视图
@available(iOS 9.0, *)
public var viewForFirstBaselineLayout: UIView { get }
/// 返回满足下基线约束条件的视图
@available(iOS 9.0, *)
public var viewForLastBaselineLayout: UIView { get }
```

##<a id="Triggering Auto Layout">13 Triggering Auto Layout

```swift
/// 更新视图和其子视图的约束
@available(iOS 6.0, *)
public func updateConstraintsIfNeeded()
/// 更新约束
@available(iOS 6.0, *)
public func updateConstraints()
/// 是否需要更新约束
@available(iOS 6.0, *)
public func needsUpdateConstraints() -> Bool
/// 约束需要更新
@available(iOS 6.0, *)
public func setNeedsUpdateConstraints()
```

##<a id="Debugging Auto Layout">14 Debugging Auto Layout

```swift
/// 返回给定方向的约束
@available(iOS 6.0, *)
public func constraintsAffectingLayoutForAxis(axis: UILayoutConstraintAxis) -> [NSLayoutConstraint]
/// 是否有模糊不清的约束
@available(iOS 6.0, *)
public func hasAmbiguousLayout() -> Bool
/// 随机使用模糊约束生效
@available(iOS 6.0, *)
public func exerciseAmbiguityInLayout()
```

##<a id="Managing the User Interface Direction">15 Managing the User Interface Direction

```swift
/// 返回视图的方向
@available(iOS 9.0, *)
public class func userInterfaceLayoutDirectionForSemanticContentAttribute(attribute: UISemanticContentAttribute) -> UIUserInterfaceLayoutDirection
// 视图改变时的布局方向
@available(iOS 9.0, *)
public var semanticContentAttribute: UISemanticContentAttribute
```

##<a id="Configuring Content Margins">16 Configuring Content Margins

```swift
/// 内容到边的间隔
@available(iOS 8.0, *)
public var layoutMargins: UIEdgeInsets
/// 间隔跟随父view
@available(iOS 8.0, *)
public var preservesSuperviewLayoutMargins: Bool
/// 到边的间隔发生改变
@available(iOS 8.0, *)
public func layoutMarginsDidChange()
```

##<a id="Drawing and Updating the View">17 Drawing and Updating the View

```swift
/// uiview绘制
public func drawRect(rect: CGRect)
/// 通知uiview需要绘制
public func setNeedsDisplay()
/// 指定位置内绘制
public func setNeedsDisplayInRect(rect: CGRect)
/// 像素点
@available(iOS 4.0, *)
public var contentScaleFactor: CGFloat
```

##<a id="Formatting Printed View Content">18 Formatting Printed View Content

```swift
/// 返回视图的打印格式化
public func viewPrintFormatter() -> UIViewPrintFormatter
/// 指定区域和打印格式绘画视图内容
public func drawRect(rect: CGRect, forViewPrintFormatter formatter: UIViewPrintFormatter)
```

##<a id="Managing Gesture Recognizers">19 Managing Gesture Recognizers

```swift
/// 所有手势识别器
@available(iOS 3.2, *)
public var gestureRecognizers: [UIGestureRecognizer]?
/// 添加手势识别器
@available(iOS 3.2, *)
public func addGestureRecognizer(gestureRecognizer: UIGestureRecognizer)
/// 移除手势识别器
@available(iOS 3.2, *)
public func removeGestureRecognizer(gestureRecognizer: UIGestureRecognizer)
/// 开始一个手势识别器
@available(iOS 6.0, *)
public func gestureRecognizerShouldBegin(gestureRecognizer: UIGestureRecognizer) -> Bool
```

##<a id="Animating Views with Block Objects">20 Animating Views with Block Objects

```swift
/// 指定时间间隔的动画
@available(iOS 4.0, *)
public class func animateWithDuration(duration: NSTimeInterval, animations: () -> Void)
/// 动画有回调
@available(iOS 4.0, *)
public class func animateWithDuration(duration: NSTimeInterval, animations: () -> Void, completion: ((Bool) -> Void)?)
// 延时执行动画
@available(iOS 4.0, *)
public class func animateWithDuration(duration: NSTimeInterval, delay: NSTimeInterval, options: UIViewAnimationOptions, animations: () -> Void, completion: ((Bool) -> Void)?)
/// 过渡动画
@available(iOS 4.0, *)
public class func transitionWithView(view: UIView, duration: NSTimeInterval, options: UIViewAnimationOptions, animations: (() -> Void)?, completion: ((Bool) -> Void)?)
/// 从一个view切换到另一个view的动画
@available(iOS 4.0, *)
public class func transitionFromView(fromView: UIView, toView: UIView, duration: NSTimeInterval, options: UIViewAnimationOptions, completion: ((Bool) -> Void)?)
/// 帧动画 可延时
@available(iOS 7.0, *)
public class func animateKeyframesWithDuration(duration: NSTimeInterval, delay: NSTimeInterval, options: UIViewKeyframeAnimationOptions, animations: () -> Void, completion: ((Bool) -> Void)?)
/// 帧动画 可持续
@available(iOS 7.0, *)
public class func addKeyframeWithRelativeStartTime(frameStartTime: Double, relativeDuration frameDuration: Double, animations: () -> Void)
/// 时间曲线对应物理动作的动画
@available(iOS 7.0, *)
public class func animateWithDuration(duration: NSTimeInterval, delay: NSTimeInterval, usingSpringWithDamping dampingRatio: CGFloat, initialSpringVelocity velocity: CGFloat, options: UIViewAnimationOptions, animations: () -> Void, completion: ((Bool) -> Void)?)
/// 系统动画
@available(iOS 7.0, *)
public class func performSystemAnimation(animation: UISystemAnimation, onViews views: [UIView], options: UIViewAnimationOptions, animations parallelAnimations: (() -> Void)?, completion: ((Bool) -> Void)?)
/// 使动画无效
@available(iOS 7.0, *)
public class func performWithoutAnimation(actionsWithoutAnimation: () -> Void)
```

##<a id="Animating Views">21 Animating Views

```swift
/// 开始动画
public class func beginAnimations(animationID: String?, context: UnsafeMutablePointer<Void>)
/// 提交动画
public class func commitAnimations()
/// 动画代理
public class func setAnimationDelegate(delegate: AnyObject?)
/// 动画开启执行的方法
public class func setAnimationWillStartSelector(selector: Selector)
/// 动画结束执行的方法
public class func setAnimationDidStopSelector(selector: Selector)
/// 动画间隔
public class func setAnimationDuration(duration: NSTimeInterval) // default = 0.2
/// 动画延时
public class func setAnimationDelay(delay: NSTimeInterval) // default = 0.0
/// 动画开始执行时间
public class func setAnimationStartDate(startDate: NSDate) // default = now ([NSDate date])
/// 放慢
public class func setAnimationCurve(curve: UIViewAnimationCurve) // default = UIViewAnimationCurveEaseInOut
/// 重复播放次数
public class func setAnimationRepeatCount(repeatCount: Float) // default = 0.0.  May be fractional
/// 动画是否自动反向
public class func setAnimationRepeatAutoreverses(repeatAutoreverses: Bool) // default = NO. used if repeat count is non-zero
/// 是否现在开始
public class func setAnimationBeginsFromCurrentState(fromCurrentState: Bool) // default = NO. If YES, the current view position is always used for new animations -- allowing animations to "pile up" on each other. Otherwise, the last end state is used for the animation (the default).
public class func setAnimationTransition(transition: UIViewAnimationTransition, forView view: UIView, cache: Bool) // current limitation - only one per begin/commit block
/// 是否激活动画
public class func setAnimationsEnabled(enabled: Bool)
/// 动画是否激活
public class func areAnimationsEnabled() -> Bool
/// 当前动画的持续时间
@available(iOS 9.0, *)
public class func inheritedAnimationDuration() -> NSTimeInterval
```

##<a id="Using Motion Effects">22 Using Motion Effects

```swift
/// 增加UIMotionEffect（运动影响视图）
@available(iOS 7.0, *)
public func addMotionEffect(effect: UIMotionEffect)
/// 移除UIMotionEffect
@available(iOS 7.0, *)
public func removeMotionEffect(effect: UIMotionEffect)
/// 获取所有UIMotionEffect
@available(iOS 7.0, *)
public var motionEffects: [UIMotionEffect]
```

##<a id="Preserving and Restoring State">23 Preserving and Restoring State

```swift
/// coder对应的标签
@available(iOS 6.0, *)
public var restorationIdentifier: String?
/// 编码视图的状态信息
@available(iOS 6.0, *)
public func encodeRestorableStateWithCoder(coder: NSCoder)
/// 解码一个视图状态信息
@available(iOS 6.0, *)
public func decodeRestorableStateWithCoder(coder: NSCoder)
```

##<a id="Capturing a View Snapshot">24 Capturing a View Snapshot

```swift
/// 获取当前视图的快照
@available(iOS 7.0, *)
public func snapshotViewAfterScreenUpdates(afterUpdates: Bool) -> UIView
/// 获取指定位置的快照
@available(iOS 7.0, *)
public func resizableSnapshotViewFromRect(rect: CGRect, afterScreenUpdates afterUpdates: Bool, withCapInsets capInsets: UIEdgeInsets) -> UIView
@available(iOS 7.0, *)
public func drawViewHierarchyInRect(rect: CGRect, afterScreenUpdates afterUpdates: Bool) -> Bool
```

##<a id="Identifying the View at Runtime">25 Identifying the View at Runtime

```swift
/// 标签
public var tag: Int
/// 根据tag获取view
public func viewWithTag(tag: Int) -> UIView? 
```

##<a id="Converting Between View Coordinate Systems">26 Converting Between View Coordinate Systems

```swift
/// 转换一个点从接受对象的坐标系到指定视图
public func convertPoint(point: CGPoint, toView view: UIView?) -> CGPoint
/// 指定视图坐标中的一个点转换为接收对象
public func convertPoint(point: CGPoint, fromView view: UIView?) -> CGPoint
/// 转换一个区域从接受对象的坐标系到指定视图
public func convertRect(rect: CGRect, toView view: UIView?) -> CGRect
/// 指定视图坐标中的一个区域转换为接收对象
public func convertRect(rect: CGRect, fromView view: UIView?) -> CGRect
```

##<a id="Hit Testing in a View">27 Hit Testing in a View

```swift
/// 在指定点上点击测试指定事件
public func hitTest(point: CGPoint, withEvent event: UIEvent?) -> UIView? // recursively calls -pointInside:withEvent:. point is in the receiver's coordinate system
/// 测试指定的点是否包含在接收对象中
public func pointInside(point: CGPoint, withEvent event: UIEvent?) -> Bool // default returns YES if point is in bounds
```

##<a id="Ending a View Editing Session">28 Ending a View Editing Session

```swift
/// 结束视图的编辑状态（关闭键盘）
public func endEditing(force: Bool) -> Bool
```

##<a id="Observing View-Related Changes">29 Observing View-Related Changes

```swift
/// did添加view
public func didAddSubview(subview: UIView)
/// will移除view
public func willRemoveSubview(subview: UIView)
/// 通知视图将要移动到一个新的父视图中
public func willMoveToSuperview(newSuperview: UIView?)
/// 通知视图已经移动到一个新的父视图中
public func didMoveToSuperview()
/// 通知视图将要移动到一个新的window中
public func willMoveToWindow(newWindow: UIWindow?)
/// 通知视图已经移动到一个新的window中
public func didMoveToWindow()
```

##<a id="Observing Focus">30 Observing Focus

```swift
/// 获取焦点
@available(iOS 9.0, *)
public func canBecomeFocused() -> Bool // NO by default
/// 是否获取焦点
@available(iOS 9.0, *)
public var focused: Bool { get }
```

&#160;

----------

#Appendix

##Related Documentation

[UIView Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html)

[View Programming Guide for iOS](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/WindowsandViews/WindowsandViews.html)

[UIKit User Interface Catalog](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/UIKitUICatalog/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-06-02 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974