[UIBarItem](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIBarItem.md)

[UIBarButtonItem](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIBarButtonItem.md)

[UITabBarItem](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UITabBarItem.md)

----

UIBarItem是一个抽象类，我们在开发过程中主要使用其子类UIBarButtonItem和UITabBarItem。

我们同样可以自定义UIBarButtonItem和UITabBarItem的展示样式，只需通过`UIBarItem.appearance()`获取共享的item即可。

#1 Getting and Setting Properties

```swift
/// 是否可点
public var enabled: Bool
/// 标题
public var title: String?
/// 图片
public var image: UIImage?
/// 横屏显示的图片
@available(iOS 5.0, *)
public var landscapeImagePhone: UIImage?
/// 图片位置
public var imageInsets: UIEdgeInsets
/// 横屏图片位置
@available(iOS 5.0, *)
public var landscapeImagePhoneInsets: UIEdgeInsets
/// 唯一标记
public var tag: Int
```

#2 Customizing Appearance

```swift
/// 根据点击状态设置不同的字体样式
@available(iOS 5.0, *)
public func setTitleTextAttributes(attributes: [String : AnyObject]?, forState state: UIControlState)
    
/// 根据点击状态获取字体样式
@available(iOS 5.0, *)
public func titleTextAttributesForState(state: UIControlState) -> [String : AnyObject]?
```

&#160;

----------

#Appendix

##Related Documentation

[UIKit Framework Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

[UITabBarController Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITabBarController_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-11 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog