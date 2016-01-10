我们在开发过程中会发现UINavigationBar并不能放太多的按钮，如右边超过2个就会很难看，此时有更多的按钮我们该怎么办呢？苹果考虑这种情况提供了UIToolbar。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016011001.jpg)

UIToolbar的位置如图所示，在屏幕的最下方，这样在一个VC中即可提供更多的按钮给用户使用。

我们还可以获取修改整个系统的UIToolbar，只需使用`UIToolbar.appearance()`拿到系统的UIToolbar，然后修改其中相关参数即可。

如果要修改当前UIToolbar，只需如下所示修改即可。

```swift
if let bar = self.navigationController?.toolbar { // 共享bar UIToolBar.appearance()
    bar.tintColor = UIColor.blackColor() // 按钮颜色
    bar.barTintColor = UIColor.yellowColor()// 背景色
}
```

#<a id="Configuring_Toolbar_Items">1 Configuring Toolbar Items

```swift
/// toolbar上的按钮
public var items: [UIBarButtonItem]?

/// 动画设置toolbar上的按钮
///
/// - parameter items : 按钮集合[UIBarButtonItem]
/// - parameter animated : Bool
///
/// - returns: void
public func setItems(items: [UIBarButtonItem]?, animated: Bool)
```

#<a id="Customizing_Appearance">2 Customizing Appearance

```swift
/// bar样式
public var barStyle: UIBarStyle
/// bar背景色
@available(iOS 7.0, *)
public var barTintColor: UIColor?
/// bar按钮颜色
public var tintColor: UIColor!
/// 是否半透明
@available(iOS 3.0, *)
public var translucent: Bool

/// 设置背景图片
@available(iOS 5.0, *)
public func setBackgroundImage(backgroundImage: UIImage?, forToolbarPosition topOrBottom: UIBarPosition, barMetrics: UIBarMetrics)
/// 获取指定位置的背景图片
@available(iOS 5.0, *)
public func backgroundImageForToolbarPosition(topOrBottom: UIBarPosition, barMetrics: UIBarMetrics) -> UIImage?
    
/// 设置阴影背景
@available(iOS 6.0, *)
public func setShadowImage(shadowImage: UIImage?, forToolbarPosition topOrBottom: UIBarPosition)
/// 获取指定位置的阴影背景
@available(iOS 6.0, *)
public func shadowImageForToolbarPosition(topOrBottom: UIBarPosition) -> UIImage?
```

#<a id="Managing_the_Delegate">3 Managing the Delegate

```swift
/// toolbar定位代理
@available(iOS 7.0, *)
unowned(unsafe) public var delegate: UIToolbarDelegate?
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UIKit Framework Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

[UINavigationController Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UINavigationController_Class/index.html)

[UIToolbar Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIToolbar_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-10 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog