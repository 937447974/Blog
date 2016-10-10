[UITabBarController](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UITabBarController.md)

[UITabBar](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UITabBar.md)

[UIBarItem](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UIBarItem.md)

[UITabBarItem](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UITabBarItem.md)

----

UITabBarItem就是UITabBar上显示的小按钮。

我们也可以定制系统UITabBarItem按钮，只需通过`UITabBarItem.appearance()`获取即可。

#1 Initializing an Item

```swift
/// 通过标题、图片、tag初始化按钮
public convenience init(title: String?, image: UIImage?, tag: Int)

/// 通过标题、默认图片和选中图片初始化按钮
@available(iOS 7.0, *)
public convenience init(title: String?, image: UIImage?, selectedImage: UIImage?)

/// 通过系统样式初始化按钮
public convenience init(tabBarSystemItem systemItem: UITabBarSystemItem, tag: Int)
```

#2 Getting and Setting Properties

```swift
/// 未读消息的标记
public var badgeValue: String?
```

#3 Customizing Appearance

```swift
/// 选中按钮的图片
@available(iOS 7.0, *)
public var selectedImage: UIImage?
    
/// 标题位置
@available(iOS 5.0, *)
public var titlePositionAdjustment: UIOffset
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