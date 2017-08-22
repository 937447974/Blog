# 1 SFSafariViewController

SFSafariViewController用于在应用中快速继承Safari浏览器的界面而无需跳出当前应用。

## 1.1 Creating a Safari View Controller

```swift
/// 代理
weak public var delegate: SFSafariViewControllerDelegate?
    
/// 初始化SFSafariViewController
public convenience init(URL: NSURL)
    
 /// 初始化SFSafariViewController，entersReaderIfAvailable是否开启阅读模式
public init(URL: NSURL, entersReaderIfAvailable: Bool)
```

# 2 实战演练

接下来实现如下所示的效果

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012101.jpg)

## 2.1 SFSafariViewController跳转

下面展示跳转的核心代码。

```swift
// MARK: - SFSafariViewController
@IBAction func onClickButton(sender: AnyObject) {
    guard let url = NSURL(string: "https://developer.apple.com/library/ios/documentation/SafariServices/Reference/SFSafariViewController_Ref/index.html") else {
        return
    }
    let vc = SFSafariViewController(URL: url, entersReaderIfAvailable: true)
    vc.delegate = self
    self.presentViewController(vc, animated: true, completion: nil)
}
```

## 2.2 实现SFSafariViewControllerDelegate

```swift
// MARK: - SFSafariViewControllerDelegate
// MARK: web页面加载完成
func safariViewController(controller: SFSafariViewController, didCompleteInitialLoad didLoadSuccessfully: Bool) {
    print(__FUNCTION__)
    print(didLoadSuccessfully)
}
    
func safariViewController(controller: SFSafariViewController, activityItemsForURL URL: NSURL, title: String?) -> [UIActivity] {
    print(__FUNCTION__)
    print("\(URL)--\(title)")
    return [YJUIActivity()]
}
    
// MARK: 退出页面
func safariViewControllerDidFinish(controller: SFSafariViewController) {
    print(__FUNCTION__)
}
```

## 2.3 类YJUIActivity

定制一个简单的UIActivity

```swift
/// 共享按钮
private class YJUIActivity: UIActivity {
    
    private override func activityTitle() -> String? {
        return "阳君"
    }
    
}
```

效果图如下所示

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012102.jpg)

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Safari Services Framework Reference](https://developer.apple.com/library/ios/documentation/SafariServices/Reference/SafariServicesFramework_Ref/index.html)

[SFSafariViewController Class Reference](https://developer.apple.com/library/ios/documentation/SafariServices/Reference/SFSafariViewController_Ref/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-21 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog