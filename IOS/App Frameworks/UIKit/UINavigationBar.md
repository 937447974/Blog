[UINavigationController](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UINavigationController.md)

[UINavigationBar](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UINavigationBar.md)

[UIToolbar](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIToolbar.md)

[UIBarItem](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIBarItem.md)

[UIBarButtonItem](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIBarButtonItem.md)

----

1. [Assigning the Delegate](#Assigning_the_Delegate)
2. [Pushing and Popping Items](#Pushing_and_Popping_Items)
3. [Customizing the Bar Appearance](#Customizing_the_Bar_Appearance)

----

UINavigationBar受UINavigationController控制，显示在屏幕的最上方，常用的有左按钮、右按钮和标题等。

使用UINavigationBar最常见的方法是结合UINavigationController一起使用，这样可以帮助我们在不同的屏幕直接快速导航。

我们还可以获取修改整个系统的UINavigationBar，只需使用`UINavigationBar.appearance()`拿到系统的UINavigationBar，然后修改其中相关参数即可。

> 值得注意的是：如果在一个UINavigationController控制的VC内修改系统UINavigationBar样式，还需要修改当前UINavigationBar。获取当前UINavigationBar的代码如下：
> 
>```swift
>if let bar = self.navigationController?.navigationBar {
>
>}
>```
 
# <a id="Assigning_the_Delegate">1 Assigning the Delegate

```swift
/// 代理监听UINavigationItem的push和pop
weak public var delegate: UINavigationBarDelegate?
```

# <a id="Pushing_and_Popping_Items">2 Pushing and Popping Items

```swift
/// 是否动画压入UINavigationItem
///
/// - parameter item : UINavigationItem
/// - parameter animated : Bool
///
/// - returns: void
public func pushNavigationItem(item: UINavigationItem, animated: Bool)
    
/// 是否动画弹出UINavigationItem
///
/// - parameter animated : Bool
///
/// - returns: 当前UINavigationItem
public func popNavigationItemAnimated(animated: Bool) -> UINavigationItem? // Returns the item that was popped.
    
/// 首个UINavigationItem
public var topItem: UINavigationItem? { get }
/// 上一个UINavigationItem
public var backItem: UINavigationItem? { get }
    
/// 所有的UINavigationItem
public var items: [UINavigationItem]?
    
/// 是否动画替换UINavigationItem
///
/// - parameter items : 要替换的[UINavigationItem]
/// - parameter animated : Bool
///
/// - returns: void
public func setItems(items: [UINavigationItem]?, animated: Bool)
```

# <a id="Customizing_the_Bar_Appearance">3 Customizing the Bar Appearance

```swift
/// 后退按钮的图片
@available(iOS 7.0, *)
public var backIndicatorImage: UIImage?
/// 跳转过程中的后退按钮图片
@available(iOS 7.0, *)
public var backIndicatorTransitionMaskImage: UIImage?
    
/// 视图样式
public var barStyle: UIBarStyle
/// 背景颜色
@available(iOS 7.0, *)
public var barTintColor: UIColor? // default is nil

/// 阴影图像
@available(iOS 6.0, *)
public var shadowImage: UIImage?
    
/// 按钮颜色
public var tintColor: UIColor!
    
/// 是否透明
@available(iOS 3.0, *)
public var translucent: Bool
    
    
/// 设置背景图片到指定位置的
@available(iOS 7.0, *)
public func setBackgroundImage(backgroundImage: UIImage?, forBarPosition barPosition: UIBarPosition, barMetrics: UIBarMetrics)
/// 获取指定位置的背景图片
public func backgroundImageForBarPosition(barPosition: UIBarPosition, barMetrics: UIBarMetrics) -> UIImage?
    
/// 设置背景图片到指定度量
@available(iOS 5.0, *)
public func setBackgroundImage(backgroundImage: UIImage?, forBarMetrics barMetrics: UIBarMetrics)
/// 获取指定度量的背景图片
@available(iOS 5.0, *)
public func backgroundImageForBarMetrics(barMetrics: UIBarMetrics) -> UIImage?
    
/// 标题样式
@available(iOS 5.0, *)
public var titleTextAttributes: [String : AnyObject]?
/// 设置标题的垂直位置
@available(iOS 5.0, *)
public func setTitleVerticalPositionAdjustment(adjustment: CGFloat, forBarMetrics barMetrics: UIBarMetrics)
/// 获取指定度量的标题位置
@available(iOS 5.0, *)
public func titleVerticalPositionAdjustmentForBarMetrics(barMetrics: UIBarMetrics) -> CGFloat
```


&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[UIKit Framework Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

[UINavigationController Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UINavigationController_Class/index.html)

[UINavigationBar Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UINavigationBar_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-09 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog