在MessageUI库中发送邮件使用MFMailComposeViewController。


#1 相关API

##1.1 Determining Mail Availability

```swift
/// 能否发送邮件
@available(iOS 3.0, *)
public class func canSendMail() -> Bool
```

##1.2 Setting Mail Fields Programmatically

```swift
/// 设置主题
@available(iOS 3.0, *)
public func setSubject(subject: String)

/// 收件人
@available(iOS 3.0, *)
public func setToRecipients(toRecipients: [String]?)

/// 抄送
@available(iOS 3.0, *)
public func setCcRecipients(ccRecipients: [String]?)

/// 密送
@available(iOS 3.0, *)
public func setBccRecipients(bccRecipients: [String]?)

/// 设置内容，可设置html格式的内容
@available(iOS 3.0, *)
public func setMessageBody(body: String, isHTML: Bool)

/// 添加附件
@available(iOS 3.0, *)
public func addAttachmentData(attachment: NSData, mimeType: String, fileName filename: String)
```

##1.3 Accessing the Delegate

```swift
/// 监听发送结果
unowned(unsafe) public var mailComposeDelegate: MFMailComposeViewControllerDelegate?
```

#2 实战演练

##2.1 源代码

```swift
//
//  ViewController.swift
//  Message
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/11.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit
import MessageUI

class ViewController: UIViewController,  MFMailComposeViewControllerDelegate, MFMessageComposeViewControllerDelegate {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    // MARK: - Action
    // MARK: compose mail
    @IBAction func composeMail(sender: AnyObject) {
        // 判断能否发送邮件
        guard MFMailComposeViewController.canSendMail() else {
            print("不能发送邮件")
            return
        }
        let mailVC = MFMailComposeViewController()
        mailVC.mailComposeDelegate = self // 代理
        mailVC.setSubject("阳君") // 主题
        mailVC.setToRecipients(["937447974@qq.com"]) // 收件人
        mailVC.setCcRecipients(["CcRecipients@qq.com"]) // 抄送
        mailVC.setBccRecipients(["bccRecipients@qq.com"]) // 密送
        mailVC.setMessageBody("相关内容", isHTML: false) // 内容，允许使用html内容
        if let image = UIImage(named: "qq") {
            if let data = UIImagePNGRepresentation(image) {
                // 添加文件
                mailVC.addAttachmentData(data, mimeType: "image/png", fileName: "qq")
            }
        }
        self.presentViewController(mailVC, animated: true, completion: nil)
    }
    
    // MARK: -  MFMailComposeViewControllerDelegate
    func mailComposeController(controller: MFMailComposeViewController, didFinishWithResult result: MFMailComposeResult, error: NSError?) {
        // 关闭MFMailComposeViewController
        controller.dismissViewControllerAnimated(true, completion: nil)
        guard error == nil else { // 错误拦截
            print(error)
            return
        }
        switch result { // 发送状态
        case MFMailComposeResultCancelled:
            print("Result: Mail sending canceled") // 删除草稿
        case MFMailComposeResultSaved: // 存储草稿
            print("Result: Mail saved")
        case MFMailComposeResultSent: // 发送成功
            print("Result: Mail sent")
        case MFMailComposeResultFailed: // 发送失败
            print("Result: Mail sending failed")
        default:// 其他
            print("Result: Mail not sent")
        }
    }
     
}

```

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016011201.jpg)
&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Message UI Framework Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

[MFMailComposeViewController Class Reference](https://developer.apple.com/library/ios/documentation/MessageUI/Reference/MFMailComposeViewController_class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-12 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog