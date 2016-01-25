1. [Presenting the Compose View](#1)
2. [Posting or Canceling a Post](#2)
3. [Validating Content](#3)
4. [Previewing Attachments](#4)
5. [Enabling Additional Configuration](#5)
6. [Enabling Text Autocompletion](#6)
7. [Accessing Content in the Compose View](#7)

----

SLComposeServiceViewController主要用于在当前应用开启分享界面，快速分享相关内容到其他APP或社交平台。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012402.jpg)

有一个可编辑文本框、一张照片、取消和确定按钮。点击确定既可将相关内容分享到我们的App中，我们还可以定制界面。

相关数据的传播经过NSExtensionContext类。

#<a id="1">1 Presenting the Compose View

```swift
/// 展示动画执行完毕
public func presentationAnimationDidFinish()
```

#<a id="2">2 Posting or Canceling a Post

```swift
/// 点击发布按钮
public func didSelectPost()
    
/// 点击取消按钮
public func didSelectCancel()
    
/// 关闭分享界面
public func cancel()
```

#<a id="3">3 Validating Content

```swift
/// 判断当前内容是否有效
public func isContentValid() -> Bool
    
/// 内容校验
public func validateContent()
    
/// 剩下可写入字符数
public var charactersRemaining: NSNumber!
```

#<a id="4">4 Previewing Attachments

```swift
/// 附近View
public func loadPreviewView() -> UIView!
```

#<a id="5">5 Enabling Additional Configuration

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

#<a id="6">6 Enabling Text Autocompletion

```swift
/// 扩展界面
public var autoCompletionViewController: UIViewController!
```

#<a id="7">7 Accessing Content in the Compose View

```swift
/// UITextView框
public var textView: UITextView! { get }

/// 内容
public var contentText: String! { get }
    
/// 默认显示
public var placeholder: String!
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Social Framework Reference](https://developer.apple.com/library/ios/documentation/Social/Reference/Social_Framework/index.html)

[SLComposeServiceViewController Class Reference](https://developer.apple.com/library/ios/documentation/Social/Reference/SLComposeServiceViewController_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-24 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog