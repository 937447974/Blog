 1. **[CNContactPickerViewController](1)**
    1. [Required Keys](#1.1)
    2. [Initializing View Controllers](#1.2)
    3. [Displaying Contact Properties](#1.3)
    4. [Notifying Delegate](#1.4)
    5. [Contact Store](#1.5)
    6. [Customizing Contact Card](#1.6)
    7. [Highlighting a Property](#1.7)
2. **[实战](#2)**
    1. [源代码](#2.1)
    2. [效果图](#2.2) 

----

#<a id="1">1 CNContactPickerViewController

CNContactPickerViewController可以显示一个联系人的相关信息、创建联系人或修改联系人。

##<a id="1.1">1.1 Required Keys

```swift
/// 获取联系人使用的描述符
public class func descriptorForRequiredKeys() -> CNKeyDescriptor
```

##<a id="1.2">1.2 Initializing View Controllers

```swift
/// 通过已有联系人初始化CNContactPickerViewController
public convenience init(forContact contact: CNContact)

/// 未知联系人初始化CNContactPickerViewController
public convenience init(forUnknownContact contact: CNContact)

/// 新建联系人初始化CNContactPickerViewController
public convenience init(forNewContact contact: CNContact?)
```

##<a id="1.3">1.3 Displaying Contact Properties

```swift
/// 联系人
public var contact: CNContact { get }
/// 所属分组
public var parentGroup: CNGroup?
/// 所属集合
public var parentContainer: CNContainer?
/// 联系人显示的名称
public var alternateName: String?
/// 相关信息
public var message: String?
/// 可显示的属性
public var displayedPropertyKeys: [AnyObject]?
```

##<a id="1.4">1.4 Notifying Delegate

```swift
/// 代理控制可显示属性，及获取修改后的联系人
weak public var delegate: CNContactViewControllerDelegate?
```

##<a id="1.5">1.5 Contact Store

```swift
/// 联系人存储库
public var contactStore: CNContactStore?
```

##<a id="1.6">1.6 Customizing Contact Card

```swift
/// 能否修改数据
public var allowsEditing: Bool // YES by default
/// 是否显示打电话、发短信等按钮
public var allowsActions: Bool // YES by default
/// 是否显示联系人的关联联系人
public var shouldShowLinkedContacts: Bool // NO by default
```

##<a id="1.7">1.7 Highlighting a Property

```swift
/// 属性高亮
public func highlightPropertyWithKey(key: String, identifier: String?)
```

#<a id="2">2 实战

这里展示创建新联系人的简单需求。

##<a id="2.1">2.1 源代码

```swift
//
//  YJContactsUIVC.swift
//  Contact
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/14.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit
import ContactsUI

/// ContactsUI显示
class YJContactsUIVC: UIViewController, CNContactViewControllerDelegate {

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    // MARK: - Action    
    // MARK: CNContactViewController 测试
    @IBAction func onClickCNContactViewController(sender: AnyObject) {
        // 创建或修改联系人
        let vc = CNContactViewController(forNewContact: nil)
        vc.delegate = self
        self.navigationController?.pushViewController(vc, animated: true)
    }
    
    // MARK: - CNContactViewControllerDelegate
    func contactViewController(viewController: CNContactViewController, shouldPerformDefaultActionForContactProperty property: CNContactProperty) -> Bool {
        print(__FUNCTION__)
        print(property)
        return true
    }
    
    func contactViewController(viewController: CNContactViewController, didCompleteWithContact contact: CNContact?) {
        viewController.navigationController?.popViewControllerAnimated(true)
        print(__FUNCTION__)
        print(contact)
    }
    

}
```

##<a id="2.2">2.2 效果图



&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[ContactsUI Framework Reference](https://developer.apple.com/library/ios/documentation/ContactsUI/Reference/ContactsUI_Framework/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-20 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog