#1 UITableViewCell

UITableViewCell定义了UITableView显示时的细胞的属性和行为。UITableViewCell提供了相关属性和方法管理cell的背景和内容（text、image、和自定义的view）、选中和高亮的状态、管理附属视图和编辑模式下的视图.

##2 搭建测试项目

测试项目的界面可自信搭建，这里使用类YJTableViewCellTVC，为了方便，它继承了UITableViewController。

```swift
//
//  YJTableViewCellTVC.swift
//  UI
//
//  Created by yangjun on 15/12/15.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// 样式
private enum YJCellStyle : Int {
    
    case Style
    case SelectionStyle
    case AccessoryType
    case SeparatorStyle
    case ManagingContentIndentation
    case SwitchTableViewCell
    
    /// 获取标题
    func title() -> String {
        var title = ""
        switch self {
        case .Style, .SelectionStyle, .AccessoryType, .SeparatorStyle:
            title = "UITableViewCell\(self)"
        case .ManagingContentIndentation:
            title = "缩进"
        case .SwitchTableViewCell:
            title = "有开关的cell"
        }
        return title
    }
    
    init(title: String) {
        switch title {
        case YJCellStyle.SelectionStyle.title():
            self = YJCellStyle.SelectionStyle
        case YJCellStyle.AccessoryType.title():
            self = YJCellStyle.AccessoryType
        case YJCellStyle.ManagingContentIndentation.title():
            self = YJCellStyle.ManagingContentIndentation
        case YJCellStyle.SwitchTableViewCell.title():
            self = YJCellStyle.SwitchTableViewCell
        default:
            self = YJCellStyle.Style
        }
    }
    
}

/// UITableViewCell
class YJTableViewCellTVC: UITableViewController {
    
    // 显示样式
    private var cellStyle = YJCellStyle.Style
    
    // MARK: - View
    override func viewDidLoad() {
        super.viewDidLoad()
        let nib = UINib(nibName: "YJSwitchTableViewCell", bundle: nil)
        self.tableView.registerNib(nib, forCellReuseIdentifier: "YJSwitchTableViewCell")
    }
    
    // MARK: - 开起和关闭tableView编辑状态
    @IBAction func onClickEdit(sender: AnyObject) {
        self.tableView.setEditing(!self.tableView.editing, animated: true)
    }
    
    // MARK: 样式展示
    @IBAction func onClickSearch(sender: AnyObject) {
        let handler: ((UIAlertAction) -> Void) = { (action: UIAlertAction) -> Void in
            self.cellStyle = YJCellStyle(title: action.title!)
            self.tableView.reloadData()
        }
        let alertController = UIAlertController(title: "样式展示", message: nil, preferredStyle: UIAlertControllerStyle.ActionSheet)
        alertController.addAction(UIAlertAction(title: YJCellStyle.Style.title(), style: .Default, handler: handler))
        alertController.addAction(UIAlertAction(title: YJCellStyle.SelectionStyle.title(), style: .Default, handler: handler))
        alertController.addAction(UIAlertAction(title: YJCellStyle.AccessoryType.title(), style: .Default, handler: handler))
        alertController.addAction(UIAlertAction(title: YJCellStyle.SeparatorStyle.title(), style: .Default, handler: { (action: UIAlertAction) -> Void in
            self.tableViewCellWithSeparatorStyle()
        }))
        alertController.addAction(UIAlertAction(title: YJCellStyle.ManagingContentIndentation.title(), style: .Default, handler: handler))
        alertController.addAction(UIAlertAction(title: YJCellStyle.SwitchTableViewCell.title(), style: .Default, handler: handler))
        alertController.addAction(UIAlertAction(title: "取消", style: .Cancel, handler: nil))
        self.presentViewController(alertController, animated: true, completion: nil)
    }
    
    // MARK: - Table view data source
    override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
        return 2
    }
    
    override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        var row = 0
        switch self.cellStyle {
        case YJCellStyle.Style, YJCellStyle.SelectionStyle, YJCellStyle.SwitchTableViewCell:
            row = 4
        case YJCellStyle.AccessoryType, YJCellStyle.ManagingContentIndentation:
            row = 5
        default:
            row = 0
        }
        return row
    }
    
    override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        switch self.cellStyle {
        case YJCellStyle.SelectionStyle:
            return self.tableViewCellWithSelectionStyle(tableView, cellForRowAtIndexPath: indexPath)
        case YJCellStyle.AccessoryType:
            return self.tableViewCellWithAccessoryType(tableView, cellForRowAtIndexPath: indexPath)
        case YJCellStyle.ManagingContentIndentation:
            return self.tableViewCellWithManagingContentIndentation(tableView, cellForRowAtIndexPath: indexPath)
        case YJCellStyle.SwitchTableViewCell:
            return self.tableViewCellWithSwitch(tableView, cellForRowAtIndexPath: indexPath)
        default:
            self.tableViewCellWithStyle(tableView, cellForRowAtIndexPath: indexPath)
        }
        return self.tableViewCellWithStyle(tableView, cellForRowAtIndexPath: indexPath)
    }
    
}
```

报红错的代码可自行注释，会在以后实现相关方法。

```swift
public init(style: UITableViewCellStyle, reuseIdentifier: String?)
```

##1.2 复用Cell

Cell可以复用，这样有助于提高内存利用率。

```swift
// 复用cell的标示符
var reuseIdentifier: String? { get }
```



![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UITableViewCell Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-16 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog