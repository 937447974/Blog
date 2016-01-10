[UITabBarController](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UITabBarController.md)

[UITabBar](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UITabBar.md)

----

1. [Setting the Delegate](#Setting_the_Delegate)
2. [Configuring Tab Bar Items](#Configuring_Tab_Bar_Items)
3. [Supporting User Customization of Tab Bars](#Supporting_User_Customization_of_Tab_Bars)
4. [Customizing Tab Bar Appearance](#Customizing_Tab_Bar_Appearance)

----

UITabBar是UITabBarController管理的控件，是系统级的VC导航控件

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016011002.jpg)

如上图所示的精品推荐、排行榜和探索等按钮就是一个UITabBar，它和UIToolBar很类似，都是处于屏幕的最下方，和UIToolBar不同的地方是它是系统级管理VC的，能够快速跳转到另一个分支的VC中。

当你想要定制UITabBar时，只需通过`UITabBar.appearance()`获取共享的UITabBar，并修改其相关属性即可。

#<a id="Setting_the_Delegate">1 Setting the Delegate

```swift
/// 相关点击代理
unowned(unsafe) public var delegate: UITabBarDelegate?
```

#<a id="Configuring_Tab_Bar_Items">2 Configuring Tab Bar Items

```swift
/// 所有UITabBarItem
public var items: [UITabBarItem]?
/// 当前选中的UITabBarItem
unowned(unsafe) public var selectedItem: UITabBarItem?
    
/// 是否动画替换[UITabBarItem]
///
/// - parameter items : [UITabBarItem]?
/// - parameter animated : Bool
///
/// - returns: void
public func setItems(items: [UITabBarItem]?, animated: Bool)
```

#<a id="Supporting_User_Customization_of_Tab_Bars">3 Supporting User Customization of Tab Bars

```swift
/// 开始定制UITabBarItem，即更多按钮中的导航
public func beginCustomizingItems(items: [UITabBarItem])
/// 结束定做[UITabBarItem]
public func endCustomizingAnimated(animated: Bool) -> Bool
/// 是否定制[UITabBarItem]
public func isCustomizing() -> Bool
```

#<a id="Customizing_Tab_Bar_Appearance">4 Customizing Tab Bar Appearance

```swift
/// bar的样色
@available(iOS 7.0, *)
public var barStyle: UIBarStyle
/// 背景色
@available(iOS 7.0, *)
public var barTintColor: UIColor? // default is nil
    
/// item的位置
@available(iOS 7.0, *)
public var itemPositioning: UITabBarItemPositioning
/// item的宽度
@available(iOS 7.0, *)
public var itemWidth: CGFloat
/// item的间距
@available(iOS 7.0, *)
public var itemSpacing: CGFloat
   
/// 按钮颜色
@available(iOS 5.0, *)
public var tintColor: UIColor!
/// 是否透明
@available(iOS 7.0, *)
public var translucent: Bool
/// 背景图片
@available(iOS 5.0, *)
public var backgroundImage: UIImage?
/// 阴影图片
@available(iOS 6.0, *)
public var shadowImage: UIImage?
/// 选中时的背景图像
@available(iOS 5.0, *)
public var selectionIndicatorImage: UIImage?
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[UIKit Framework Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

[UITabBarController Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITabBarController_Class/index.html)

[UITabBar Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITabBar_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-07 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog