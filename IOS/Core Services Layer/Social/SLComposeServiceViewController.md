**1 [SLComposeServiceViewController](#1)**

1. [Presenting the Compose View](#1.1)
2. [Posting or Canceling a Post](#1.2)
3. [Validating Content](#1.3)
4. [Previewing Attachments](#1.4)
5. [Enabling Additional Configuration](#1.5)
6. [Enabling Text Autocompletion](#1.6)
7. [Accessing Content in the Compose View](#1.7)

**2 [SLComposeSheetConfigurationItem](#2)**

1. [Responding to User Interaction](#2.1)
2. [Specifying Configuration Information](#2.2)
3. [Getting a Configuration Item](#2.3)

----

#<a id="1">1 SLComposeServiceViewController

SLComposeServiceViewController可以在共享平台将其他应用的数据共享到我们的应用中，如下图所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012402.jpg)

有一个可编辑文本框、一张照片、取消和确定按钮。点击确定既可将相关内容分享到我们的App中，我们还可以定制界面。

相关数据的传播经过NSExtensionContext类。

##<a id="1.1">1 Presenting the Compose View

```swift
/// 展示动画执行完毕
public func presentationAnimationDidFinish()
```

##<a id="1.2">2 Posting or Canceling a Post

```swift
/// 点击发布按钮
public func didSelectPost()
    
/// 点击取消按钮
public func didSelectCancel()
    
/// 关闭分享界面
public func cancel()
```

##<a id="1.3">3 Validating Content

```swift
/// 判断当前内容是否有效
public func isContentValid() -> Bool
    
/// 内容校验
public func validateContent()
    
/// 剩下可写入字符数
public var charactersRemaining: NSNumber!
```

##<a id="1.4">4 Previewing Attachments

```swift
/// 附近View
public func loadPreviewView() -> UIView!
```

##<a id="1.5">5 Enabling Additional Configuration

```swift
/// 配置[SLComposeSheetConfigurationItem]
public func configurationItems() -> [AnyObject]!
    
/// 刷新[SLComposeSheetConfigurationItem]
public func reloadConfigurationItems()

/// 进入下一个视图
public func pushConfigurationViewController(viewController: UIViewController!)
    
/// 回到上个视图
public func popConfigurationViewController()
```

##<a id="1.6">6 Enabling Text Autocompletion

```swift
/// 扩展界面
public var autoCompletionViewController: UIViewController!
```

##<a id="1.7">7 Accessing Content in the Compose View

```swift
/// UITextView框
public var textView: UITextView! { get }

/// 内容
public var contentText: String! { get }
    
/// 默认显示
public var placeholder: String!
```

#<a id="2">2 SLComposeSheetConfigurationItem

SLComposeSheetConfigurationItem主要是在SLComposeServiceViewController分享扩展界面添加选择条目时使用如图中的"选择"就是SLComposeSheetConfigurationItem控件。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012501.jpg)

##<a id="2.1">2.1 Responding to User Interaction

```swift
/// 用户点击列表回调
public var tapHandler: SLComposeSheetConfigurationItemTapHandler!
```

##<a id="2.2">2.2 Specifying Configuration Information

```swift
/// 左标题
public var title: String!
/// 右描述
public var value: String!
/// 是否开启转圈模式
public var valuePending: Bool
```

##<a id="2.3">2.3 Getting a Configuration Item

```swift
/// 初始化
public init!()
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Social Framework Reference](https://developer.apple.com/library/ios/documentation/Social/Reference/Social_Framework/index.html)

[SLComposeServiceViewController Class Reference](https://developer.apple.com/library/ios/documentation/Social/Reference/SLComposeServiceViewController_Class/index.html)

[SLComposeSheetConfigurationItem Class Reference](https://developer.apple.com/library/ios/documentation/Social/Reference/SLComposeSheetConfigurationItem_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-24 | 博文完成 |
| 2016-01-25 | 添加SLComposeSheetConfigurationItem |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog