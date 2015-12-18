[Auto Layout(NSLayoutConstraint)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/Auto%20Layout(NSLayoutConstraint).md)

[Auto Layout(NSLayoutAnchor)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/Auto%20Layout(NSLayoutAnchor).md)

[Auto Layout(Storyboard)](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/Auto%20Layout(Storyboard).md)

---

在IPhone 6出来之后，作为开发人员不得不考虑一件事了，那就是屏幕适配。曾经这是安卓开发人员头疼的事，现在也让苹果开发人员头疼了。

苹果基于不同屏幕的考虑，支持大家去做兼容性开发。所使用的类是NSLayoutConstraint，这帮助我们在基于不同的屏幕开发同一套开发语言。

#1 NSLayoutConstraint

NSLayoutConstraint就是我们屏幕适配的核心类，你可以通过动态修改它内部数据以达到修改UI的目的。

##1.1 创建约束

```swift
// 连续创建多个约束
public class func constraintsWithVisualFormat(format: String, options opts: NSLayoutFormatOptions, metrics: [String : AnyObject]?, views: [String : AnyObject]) -> [NSLayoutConstraint]

// 基于两个View创建约束，也可以约束一个View的宽、高
public convenience init(item view1: AnyObject, attribute attr1: NSLayoutAttribute, relatedBy relation: NSLayoutRelation, toItem view2: AnyObject?, attribute attr2: NSLayoutAttribute, multiplier: CGFloat, constant c: CGFloat)
```

创建约束的规则有两点，在UI上，

1. 左边的View对应view1，右边的View对应view2；
2. 下面的View对应View1，上面的View对应View2。

口诀就是：从右下到左上

生成的公式就是：

```swift
firstItem.firstAttribute {==,<=,>=} secondItem.secondAttribute * multiplier + constant
```

##1.2 启用和禁用约束

```swift
// 启用约束
public var active: Bool

// 启用多个约束
public class func activateConstraints(constraints: [NSLayoutConstraint])

// 禁用多个约束
public class func deactivateConstraints(constraints: [NSLayoutConstraint])
```

##1.3 访问约束数据

```swift
// 约束的优先级
public var priority: UILayoutPriority
// 左或上View
unowned(unsafe) public var firstItem: AnyObject { get }
// firstItem对应的约束属性
public var firstAttribute: NSLayoutAttribute { get }
// 两个约束的关系{==,<=,>=}
public var relation: NSLayoutRelation { get }
// 右或下View
unowned(unsafe) public var secondItem: AnyObject? { get }
// secondItem对应的约束属性
public var secondAttribute: NSLayoutAttribute { get }
// 比较级，默认1.0
public var multiplier: CGFloat { get }
// 约束条件下的常量位移
public var constant: CGFloat
```

##1.4 识别约束

```swift
// 约束的唯一标示
public var identifier: String?
```

##1.5 控制约束归档

```swift
// 约束是否归档
public var shouldBeArchived: Bool
```

#2 常量

##2.1 NSLayoutRelation

```swift
public enum NSLayoutRelation : Int {
    case LessThanOrEqual
    case Equal
    case GreaterThanOrEqual
}
```

##2.2 NSLayoutAttribute

```swift
public enum NSLayoutAttribute : Int {
    
    case Left
    case Right
    case Top
    case Bottom
    case Leading
    case Trailing
    case Width
    case Height
    case CenterX
    case CenterY
    case Baseline
    public static var LastBaseline: NSLayoutAttribute { get }
    @available(iOS 8.0, *)
    case FirstBaseline
    
    @available(iOS 8.0, *)
    case LeftMargin
    @available(iOS 8.0, *)
    case RightMargin
    @available(iOS 8.0, *)
    case TopMargin
    @available(iOS 8.0, *)
    case BottomMargin
    @available(iOS 8.0, *)
    case LeadingMargin
    @available(iOS 8.0, *)
    case TrailingMargin
    @available(iOS 8.0, *)
    case CenterXWithinMargins
    @available(iOS 8.0, *)
    case CenterYWithinMargins
    
    case NotAnAttribute
}
```

#3 实战演练

##3.1 需求效果图



![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[Auto Layout Guide](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/LayoutUsingStackViews.html)

[NSLayoutConstraint Class Reference](https://developer.apple.com/library/ios/documentation/AppKit/Reference/NSLayoutConstraint_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-18 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog