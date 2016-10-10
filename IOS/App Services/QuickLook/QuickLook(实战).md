前面已经讲解了QuickLook相关知识，接下来使用QuickLook库预览pdf文件。

#1 显示的文件

显示的文件需要自定义实体，然后继承QLPreviewItem。这里制作一个简单的文件实体类。

```swift
//
//  YJPreviewItem.swift
//  YJQuickLook
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/24.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit
import QuickLook

/// 文件QLPreviewItem
class YJPreviewItem: NSObject, QLPreviewItem {
    
    var previewItemURL: NSURL {
        return  NSBundle.mainBundle().URLForResource("Test", withExtension: "pdf")!
    }
    
    var previewItemTitle: String? {
        return "Title"
    }
    
}
```

我们使用Test.pdf文件，并且这个文件已经导入到项目中。你也可以使用其他文件。

#2 页面跳转

当用户点击屏幕时发生跳转，具体代码如下。

```swift
//
//  ViewController.swift
//  YJQuickLook
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/24.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit
import QuickLook

class ViewController: UIViewController, QLPreviewControllerDataSource {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
    
    // MARK: - 点击屏幕
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        super.touchesBegan(touches, withEvent: event)
        let qlVC = QLPreviewController()
        qlVC.dataSource = self
        qlVC.currentPreviewItemIndex = 0 // 显示第一个
        self.presentViewController(qlVC, animated: true, completion: nil)
    }
    
}
```

#3 实现QLPreviewControllerDataSource

要显示的item需要实现QLPreviewControllerDataSource协议展示。

```swift
// MARK: - QLPreviewControllerDataSource
func numberOfPreviewItemsInPreviewController(controller: QLPreviewController) -> Int {
    print(__FUNCTION__)
    return 2
}
    
func previewController(controller: QLPreviewController, previewItemAtIndex index: Int) -> QLPreviewItem {
    print(__FUNCTION__)
    return YJPreviewItem()
}
```

#4 效果图

运行项目点击屏幕即可看见如下效果图。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012401.jpg)

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Quick Look Framework Reference for iOS](https://developer.apple.com/library/ios/documentation/QuickLook/Reference/QuickLookFrameworkReference_iPhoneOS/index.html)

[QLPreviewController Class Reference](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/QLPreviewController_Class/index.html)

[QLPreviewControllerDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/QLPreviewControllerDelegate_Protocol/index.html)

[QLPreviewControllerDataSource Protocol Reference](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/QLPreviewControllerDataSource_Protocol/index.html)

[QLPreviewItem Protocol Reference for iOS](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/QLPreviewItem_Protocol_iPhoneOS/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-24 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog