
**1 [CNContactPickerViewController](#1)**

1.1 [Displaying Contacts Properties](#1.1)

1.2 [Notifying Delegate](#1.2)

1.3 [Predicates For Selecting Contacts](#1.3)

**2 [实战演练](#2)**

2.1 [源代码](#2.1)

2.2 [效果图](#2.2)

----

#<a id="">1 CNContactPickerViewController

CNContactPickerViewController支持快速选中联系人或联系人的属性，支持单选和多选。

##<a id="1.1">1.1 Displaying Contacts Properties

```swift
/// 联系人名片可展示的信息
public var displayedPropertyKeys: [String]?
```

##<a id="1.2">1.2 Notifying Delegate

```swift
/// 通过代理控制选择的联系人或联系人属性
weak public var delegate: CNContactPickerDelegate?
```

##<a id="1.3">1.3 Predicates For Selecting Contacts

```swift
/// 晒选联系人
@NSCopying public var predicateForEnablingContact: NSPredicate? // e.g. emailAddresses.@count > 0

/// 可选得联系人
@NSCopying public var predicateForSelectionOfContact: NSPredicate? // e.g. emailAddresses.@count == 1

/// 可选的联系人属性
@NSCopying public var predicateForSelectionOfProperty: NSPredicate? // e.g. (key == 'emailAddresses') AND (value LIKE '*@apple.com')
```

#<a id="2">2 实战演练

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
class YJContactsUIVC: UIViewController, CNContactPickerDelegate {

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    // MARK: - Action
    // MARK: CNContactPickerViewController 测试
    @IBAction func onClickCNContactPickerViewController(sender: AnyObject) {
        // 选择联系人或联系人的属性（如电话号码）
        let vc = CNContactPickerViewController()
        vc.delegate = self // 根据实现的代理方法指定单选、多选
        self.presentViewController(vc, animated: true, completion: nil)
    }
    
    // MARK: - CNContactPickerDelegate
    func contactPicker(picker: CNContactPickerViewController, didSelectContact contact: CNContact) {
        // 单选
        print(contact)
    }
    
}
```

##<a id="2.2">2.2 效果图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012003.jpg)

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Contacts Framework Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/Contacts_Framework/index.html)

[CNContactPickerViewController Class Reference](https://developer.apple.com/library/ios/documentation/ContactsUI/Reference/CNContactPickerViewController_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-20 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog