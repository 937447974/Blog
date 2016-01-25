**1 [SLComposeViewController](#1)**

1. [Creating a Social Compose View Controller](#1.1)
2. [Checking the Social Service Type](#1.2)
3. [Composing Posts](#1.3)
4. [Handling Results](#1.4)

**2 [实战演练](#2)**

----

#<a id="1">1 SLComposeViewController

SLComposeViewController主要用于在当前应用开启分享界面，快速分享相关内容到其他APP或社交平台。如下所示

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012501.jpg)

##<a id="1.1">1.1 Creating a Social Compose View Controller

```swift
/// 根据扩展的Bundle identifier初始化
public init!(forServiceType serviceType: String!)
```

##<a id="1.2">1.2 Checking the Social Service Type

```swift
/// 判断该分享扩展是否支持
public class func isAvailableForServiceType(serviceType: String!) -> Bool
    
/// 获取分享扩展的类型编码
public var serviceType: String! { get }
```

##<a id="1.3">1.3 Composing Posts

```swift
/// 设置默认内容
public func setInitialText(text: String!) -> Bool
    
/// 添加图片
public func addImage(image: UIImage!) -> Bool
    
/// 清空所有图片
public func removeAllImages() -> Bool
    
/// 添加url链接
public func addURL(url: NSURL!) -> Bool
    
/// 清空所有链接
public func removeAllURLs() -> Bool
```

##<a id="1.4">1.4 Handling Results

```swift
/// 分享结果监听
public var completionHandler: SLComposeViewControllerCompletionHandler!
```

#<a id="2">2 实战演练

应用内打开分享扩展很简单，只需如下所示。

```swift
//
//  ViewController.swift
//  YJSocial
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/24.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit
import Social

/// 快速分享
class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
    
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        super.touchesBegan(touches, withEvent: event)
        // 分享到当前应用扩展（微博分享SLServiceTypeSinaWeibo）
        let serviceType = "com.YJSocial.ShareExtension" // 扩展Bundle identifier
        guard SLComposeViewController.isAvailableForServiceType(serviceType) else {
            print("不支持:\(serviceType)")
            return
        }
        let vc = SLComposeViewController(forServiceType: serviceType)
        vc.setInitialText(serviceType) // 默认内容
        // 处理结果回调
        vc.completionHandler =  {(result: SLComposeViewControllerResult) -> Void in
            switch result {
            case SLComposeViewControllerResult.Cancelled:
                print("Cancelled")
            case SLComposeViewControllerResult.Done:
                print("Done")
            }
        }
        self.presentViewController(vc, animated: true, completion: nil)
    }
    
}
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Social Framework Reference](https://developer.apple.com/library/ios/documentation/Social/Reference/Social_Framework/index.html)

[SLComposeViewController Class Reference](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/SLComposeViewController_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-25 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog