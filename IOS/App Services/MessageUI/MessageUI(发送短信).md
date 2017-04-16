在MessageUI库中发送短信使用MFMessageComposeViewController。

# 1 相关API

## 1.1 Determining If Message Composition Is Available

```swift
/// 能否发短信
///
/// - returns: Bool
public class func canSendText() -> Bool
    
/// 能否发主题
///
/// - returns: Bool
@available(iOS 7.0, *)
public class func canSendSubject() -> Bool
    
/// 能否发附件
///
/// - returns: Bool
@available(iOS 7.0, *)
public class func canSendAttachments() -> Bool
    
/// 附件的UTI支持
///
/// - parameter uti :  See [Uniform Type Identifiers Reference](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/UTIRef/Introduction/Introduction.html)
///
/// - returns: Bool
@available(iOS 7.0, *)
public class func isSupportedAttachmentUTI(uti: String) -> Bool
```

## 1.2 Accessing the Delegate

```swift
/// 监听发送结果
unowned(unsafe) public var messageComposeDelegate: MFMessageComposeViewControllerDelegate?
```

## 1.3 Setting the Initial Message Information

```swift
/// 取消添加附件的按钮
@available(iOS 7.0, *)
public func disableUserAttachments()
    
/// 收件人
public var recipients: [String]?
/// 内容
public var body: String?
/// 主题
public var subject: String?
/// 所有附件
public var attachments: [[NSObject : AnyObject]]? { get }
    
/// 添加url类型的附件
@available(iOS 7.0, *)
public func addAttachmentURL(attachmentURL: NSURL, withAlternateFilename alternateFilename: String?) -> Bool
    
/// 添加NSData数据的附件
@available(iOS 7.0, *)
public func addAttachmentData(attachmentData: NSData, typeIdentifier uti: String, filename: String) -> Bool
```

# 2 实战演练

## 2.1 源代码

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
    
    // MARK: compose message
    @IBAction func composeMessage(sender: AnyObject) {
        guard MFMessageComposeViewController.canSendText() else {
            print("不能发送短信")
            return
        }
        let messageVC = MFMessageComposeViewController()
        messageVC.messageComposeDelegate = self // 代理
        messageVC.recipients = ["18511056826"] // 收件人
        messageVC.body = "短信内容" // 内容
        // 发送主题
        if MFMessageComposeViewController.canSendSubject() {
            messageVC.subject = "阳君"
        }
        // 发送附件
        if MFMessageComposeViewController.canSendAttachments() {
            // 路径添加
            if let path = NSBundle.mainBundle().pathForResource("Info", ofType: "plist") {
                messageVC.addAttachmentURL(NSURL(fileURLWithPath: path), withAlternateFilename: "Info.plist")
            }
            // NSData添加
            if MFMessageComposeViewController.isSupportedAttachmentUTI("public.png") {
                if let image = UIImage(named: "qq") {
                    if let data = UIImagePNGRepresentation(image) {
                        // 添加文件
                        messageVC.addAttachmentData(data, typeIdentifier: "public.png", filename: "qq.png")
                    }
                }
            }
            print(messageVC.attachments) // 所有附件
        }
        // messageVC.disableUserAttachments() // 禁用添加附件按钮
        self.presentViewController(messageVC, animated: true, completion: nil)
    }
    
    // MARK: - MFMessageComposeViewControllerDelegate
    func messageComposeViewController(controller: MFMessageComposeViewController, didFinishWithResult result: MessageComposeResult) {
        // 关闭MFMessageComposeViewController
        controller.dismissViewControllerAnimated(true, completion: nil)
        switch result { // 发送状态
        case MessageComposeResultCancelled:
            print("Result: Mail sending cancelled") // 取消发送
        case MessageComposeResultSent: // 发送成功
            print("Result: Mail sent")
        case MessageComposeResultFailed: // 发送失败
            print("Result: Message sending failed")
        default:// 其他
            print("Result: Message not sent")
        }
    }
    
}
```

## 2.2 效果图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016011202.jpg)

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Message UI Framework Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-12 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog