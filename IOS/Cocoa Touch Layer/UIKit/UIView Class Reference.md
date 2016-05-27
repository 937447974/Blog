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
/// 子view改变layout，更精确的控制
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
```

##<a id="Triggering Auto Layout">13 Triggering Auto Layout

```swift
```

##<a id="Debugging Auto Layout">14 Debugging Auto Layout

```swift
```

##<a id="Managing the User Interface Direction">15 Managing the User Interface Direction

```swift
```

##<a id="Configuring Content Margins">16 Configuring Content Margins

```swift
```

##<a id="Drawing and Updating the View">17 Drawing and Updating the View

```swift
```

##<a id="Formatting Printed View Content">18 Formatting Printed View Content

```swift
```

##<a id="Managing Gesture Recognizers">19 Managing Gesture Recognizers

```swift
```

##<a id="Animating Views with Block Objects">20 Animating Views with Block Objects

```swift
```

##<a id="Animating Views">21 Animating Views

```swift
```

##<a id="Using Motion Effects">22 Using Motion Effects

```swift
```

##<a id="Preserving and Restoring State">23 Preserving and Restoring State

```swift
```

##<a id="Capturing a View Snapshot">24 Capturing a View Snapshot

```swift
```

##<a id="Identifying the View at Runtime">25 Identifying the View at Runtime

```swift
```

##<a id="Converting Between View Coordinate Systems">26 Converting Between View Coordinate Systems

```swift
```

##<a id="Hit Testing in a View">27 Hit Testing in a View

```swift
```

##<a id="Ending a View Editing Session">28 Ending a View Editing Session

```swift
```

##<a id="Observing View-Related Changes">29 Observing View-Related Changes

```swift
```

##<a id="Observing Focus">30 Observing Focus

```swift
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
| 2016-03-20 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974