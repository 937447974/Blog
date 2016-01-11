[UIBarItem](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIBarItem.md)

[UIBarButtonItem](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIBarButtonItem.md)

[UINavigationController](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UINavigationController.md)

[UINavigationBar](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UINavigationBar.md)

[UIToolbar](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIToolbar.md)

----

1. [Initializing an Item](#Initializing_an_Item)
2. [Getting and Setting Properties](#Getting_and_Setting_Properties)
3. [Customizing Appearance](#Customizing_Appearance)
4. [Getting the Shortcuts Group Information](#Getting_the_Shortcuts_Group_Information)

----

UIBarButtonItem就是我们在UIToolbar和UINavigationBar上看见的按钮。

如果你要改变UIBarButtonItem的全局样式，可以通过`UIBarButtonItem.appearance()`获取全局UIBarButtonItem。

#<a id="Initializing_an_Item">1 Initializing an Item

```swift
/// 通过图片初始化UIBarButtonItem
public convenience init(image: UIImage?, style: UIBarButtonItemStyle, target: AnyObject?, action: Selector)
    
/// 通过图片初始化UIBarButtonItem，横竖屏可不一致
@available(iOS 5.0, *)
public convenience init(image: UIImage?, landscapeImagePhone: UIImage?, style: UIBarButtonItemStyle, target: AnyObject?, action: Selector)

/// 通过文字初始化UIBarButtonItem
public convenience init(title: String?, style: UIBarButtonItemStyle, target: AnyObject?, action: Selector)

/// 通过系统样式初始化UIBarButtonItem
public convenience init(barButtonSystemItem systemItem: UIBarButtonSystemItem, target: AnyObject?, action: Selector)

/// 通过自定义View初始化UIBarButtonItem
public convenience init(customView: UIView)
```

#<a id="Getting_and_Setting_Properties">2 Getting and Setting Properties

```swift
/// 系统点击样式
public var style: UIBarButtonItemStyle
/// 按钮宽
public var width: CGFloat
/// 可能显示的标题
public var possibleTitles: Set<String>?
/// 按钮自定义的UIView
public var customView: UIView?
/// 点击按钮的接收器
public var action: Selector
/// 点击按钮执行的方法
weak public var target: AnyObject?
```

#<a id="Customizing_Appearance">3 Customizing Appearance

```swift
/// 设置背景图片
@available(iOS 5.0, *)
public func setBackgroundImage(backgroundImage: UIImage?, forState state: UIControlState, barMetrics: UIBarMetrics)
/// 获取背景图片
@available(iOS 5.0, *)
public func backgroundImageForState(state: UIControlState, barMetrics: UIBarMetrics) -> UIImage?
    
/// 设置背景图片
@available(iOS 6.0, *)
public func setBackgroundImage(backgroundImage: UIImage?, forState state: UIControlState, style: UIBarButtonItemStyle, barMetrics: UIBarMetrics)
@available(iOS 6.0, *)
/// 获取背景图片
public func backgroundImageForState(state: UIControlState, style: UIBarButtonItemStyle, barMetrics: UIBarMetrics) -> UIImage?
    
/// 按钮颜色
@available(iOS 5.0, *)
public var tintColor: UIColor?
    
/// 调整垂直方向的位置
@available(iOS 5.0, *)
public func setBackgroundVerticalPositionAdjustment(adjustment: CGFloat, forBarMetrics barMetrics: UIBarMetrics)
/// 获取垂直方向的位置
@available(iOS 5.0, *)
public func backgroundVerticalPositionAdjustmentForBarMetrics(barMetrics: UIBarMetrics) -> CGFloat
    
/// 设置标题位置
@available(iOS 5.0, *)
public func setTitlePositionAdjustment(adjustment: UIOffset, forBarMetrics barMetrics: UIBarMetrics)
/// 获取标题位置
@available(iOS 5.0, *)
public func titlePositionAdjustmentForBarMetrics(barMetrics: UIBarMetrics) -> UIOffset
    
    
// MARK: - 后退按钮
    
/// 后退按钮的图片
@available(iOS 5.0, *)
public func setBackButtonBackgroundImage(backgroundImage: UIImage?, forState state: UIControlState, barMetrics: UIBarMetrics)
/// 获取后退按钮的图片
@available(iOS 5.0, *)
public func backButtonBackgroundImageForState(state: UIControlState, barMetrics: UIBarMetrics) -> UIImage?
    
/// 设置后退按钮的字体样式
@available(iOS 5.0, *)
public func setBackButtonTitlePositionAdjustment(adjustment: UIOffset, forBarMetrics barMetrics: UIBarMetrics)
/// 获取后退按钮的字体样式
@available(iOS 5.0, *)
public func backButtonTitlePositionAdjustmentForBarMetrics(barMetrics: UIBarMetrics) -> UIOffset
    
/// 设置后退按钮标题位置
@available(iOS 5.0, *)
public func setBackButtonBackgroundVerticalPositionAdjustment(adjustment: CGFloat, forBarMetrics barMetrics: UIBarMetrics)
/// 获取后退按钮标题位置
@available(iOS 5.0, *)
public func backButtonBackgroundVerticalPositionAdjustmentForBarMetrics(barMetrics: UIBarMetrics) -> CGFloat
```

#<a id="Getting_the_Shortcuts_Group_Information">4 Getting the Shortcuts Group Information

```swift
/// 按钮管理的按钮组
@available(iOS 9.0, *)
weak public var buttonGroup: UIBarButtonItemGroup? { get }
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[UIKit Framework Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

[UIBarButtonItem Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIBarButtonItem_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-11 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog