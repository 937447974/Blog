下面将介绍使用NotificationCenter库实现如下效果图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012001.jpg)

需求如下：

1. 显示TodayExtension；
2. 点击Button按钮打开主APP；
3. 和主APP共享数据

#1 显示TodayExtension

##1.1 创建项目

和常规创建项目一样，你也可以使用你目前的项目，这里不做详细说明。

##1.2 创建Today Extension

打开项目后，点击Editor-Add Target

如下所示

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012004.png)
//
//  TodayViewController.swift
//  TodayExtension
//
//  Created by yangjun on 16/1/19.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit
import NotificationCenter

class TodayViewController: UIViewController, NCWidgetProviding {
    
    @IBOutlet weak var button: UIButton!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view from its nib.
        // 隐藏或显示界面
        NCWidgetController.widgetController().setHasContent(true, forWidgetWithBundleIdentifier: YJUtilsAPP.identifier)
        print(YJUtilsAPP.identifier)
        // 调整显示区域
        self.preferredContentSize = CGSizeMake(self.view.frame.width, 50)
        print(self.view.frame)
    }
    
    override func viewWillAppear(animated: Bool) {
        super.viewDidAppear(animated)
        print(__FUNCTION__)
    }
    
    override func viewDidAppear(animated: Bool) {
        super.viewDidAppear(animated)
        print(__FUNCTION__)
    }
    
    deinit {
        print("内存回收")
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    @IBAction func onClickButton(sender: AnyObject) {
        print(__FUNCTION__)
        // 数据同步
        if let user = NSUserDefaults(suiteName: "group.com.YJNotificationCenter") {
            print(user.objectForKey("test"))
            user.setObject("阳君测试", forKey: "test")
            print(user.synchronize())
        }
        // 打开当前应用
        if let url = NSURL(string: "appextension://test") {
            self.extensionContext?.openURL(url, completionHandler: { (success: Bool) -> Void in
                print("open url result: \(success)")
            })
        }
    }
    
    // MARK: - NCWidgetProviding
    // MARK: 当前状态
    func widgetPerformUpdateWithCompletionHandler(completionHandler: ((NCUpdateResult) -> Void)) {
        // Perform any setup necessary in order to update the view.
        // If an error is encountered, use NCUpdateResult.Failed
        // If there's no update required, use NCUpdateResult.NoData
        // If there's an update, use NCUpdateResult.NewData
        print(__FUNCTION__)
        completionHandler(NCUpdateResult.NewData)
    }
    
    // MARK: 扩展显示边缘
    func widgetMarginInsetsForProposedMarginInsets(defaultMarginInsets: UIEdgeInsets) -> UIEdgeInsets {
        print(__FUNCTION__)
        print(defaultMarginInsets)
        return UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
    }
    
}

运行即可看见效果图。

#2 点击Button按钮打开主APP

修改布局文件，添加button按钮，并修改源代码，如下。

```swift
//
//  TodayViewController.swift
//  TodayExtension
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/19.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit
import NotificationCenter

class TodayViewController: UIViewController, NCWidgetProviding {
        
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view from its nib.
        // 隐藏或显示界面
        NCWidgetController.widgetController().setHasContent(true, forWidgetWithBundleIdentifier: YJUtilsAPP.identifier)
        print(YJUtilsAPP.identifier)
        // 调整显示区域
        self.preferredContentSize = CGSizeMake(self.view.frame.width, 50)
        print(self.view.frame)
    }
    
    @IBAction func onClickButton(sender: AnyObject) {
        print(__FUNCTION__)
    
    }
    
    // MARK: - NCWidgetProviding
    // MARK: 当前状态
    func widgetPerformUpdateWithCompletionHandler(completionHandler: ((NCUpdateResult) -> Void)) {
        // Perform any setup necessary in order to update the view.
        // If an error is encountered, use NCUpdateResult.Failed
        // If there's no update required, use NCUpdateResult.NoData
        // If there's an update, use NCUpdateResult.NewData
        print(__FUNCTION__)
        completionHandler(NCUpdateResult.NewData)
    }
    
    // MARK: 扩展显示边缘
    func widgetMarginInsetsForProposedMarginInsets(defaultMarginInsets: UIEdgeInsets) -> UIEdgeInsets {
        print(__FUNCTION__)
        print(defaultMarginInsets)
        return UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
    }
    
}
```

在onClickButton方法添加如下代码即可跳转

```swift
// 打开当前应用
if let url = NSURL(string: "appextension://test") {
    self.extensionContext?.openURL(url, completionHandler: { (success: Bool) -> Void in
        print("open url result: \(success)")
    })
}
```



&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Notification Center Framework Reference](https://developer.apple.com/library/ios/documentation/NotificationCenter/Reference/NotificationCenter_Framework/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-20 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog