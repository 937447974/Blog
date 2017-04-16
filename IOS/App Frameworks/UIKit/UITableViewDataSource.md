UITableViewDataSource是UITableView的一个协议，它主要帮助我们设置UITableView显示的数据源。

作为数据模型的协议，数据源提供了表视图的最小信息，UITableView显示的每个Cell都是通过UITableViewDataSource提供的。

UITableViewDataSource要求我们提供显示UITableViewCell，以及要显示的cell个数，你还可以使他们以组的形式显示。

# 1 配置UItableVIew

```swift

// 有几组
@available(iOS 2.0, *)
optional public func numberOfSectionsInTableView(tableView: UITableView) -> Int 

// 每一组显示的cell个数
@available(iOS 2.0, *)
public func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int

// 生成每一个Cell    
@available(iOS 2.0, *)
public func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell

// 组Header
@available(iOS 2.0, *)
optional public func tableView(tableView: UITableView, titleForHeaderInSection section: Int) -> String?
// 组Footer
@available(iOS 2.0, *)
optional public func tableView(tableView: UITableView, titleForFooterInSection section: Int) -> String?

// 索引
@available(iOS 2.0, *)
optional public func sectionIndexTitlesForTableView(tableView: UITableView) -> [String]? 

// 索引对应的组
@available(iOS 2.0, *)
optional public func tableView(tableView: UITableView, sectionForSectionIndexTitle title: String, atIndex index: Int) -> Int
```

# 2 插入或删除行


```swift
// 能否进入编辑模式
@available(iOS 2.0, *)
optional public func tableView(tableView: UITableView, canEditRowAtIndexPath indexPath: NSIndexPath) -> Bool

// 增加或删除行
@available(iOS 2.0, *)
optional public func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath)
```

# 3 移动UITableViewCell

```swift
// 能否移动
@available(iOS 2.0, *)
optional public func tableView(tableView: UITableView, canMoveRowAtIndexPath indexPath: NSIndexPath) -> Bool

// 移动某行
@available(iOS 2.0, *)
optional public func tableView(tableView: UITableView, moveRowAtIndexPath sourceIndexPath: NSIndexPath, toIndexPath destinationIndexPath: NSIndexPath)
```

# 4 示例代码

这里使用类YJTableViewDataSourceVC，至于界面请自行搭建。

```swift
//
//  YJTableViewDataSourceVC.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/14.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// TableViewDataSource展示
class YJTableViewDataSourceVC: UIViewController, UITableViewDataSource {
    
    /// 数据源
    var data = [[Int]]()
    /// UITableView
    @IBOutlet weak var tableView: UITableView!
    
    // MARK: - view
    override func viewDidLoad() {
        super.viewDidLoad()
        // 以组演示,填充相关测试数据
        var section = [Int]()
        for _ in 0..<5 {
            section.removeAll()
            for row in 0..<10 {
                section.append(row)
            }
            self.data.append(section)
        }
    }
    
    // MARK: - 开起和关闭tableView编辑状态
    @IBAction func onClickEdit(sender: AnyObject) {
        self.tableView.editing = !self.tableView.editing
    }
    
    // MARK: - UITableViewDataSource
    // MARK: 有几组
    func numberOfSectionsInTableView(tableView: UITableView) -> Int  {
        print(__FUNCTION__)
        return self.data.count
    }
    
    // MARK: 每一组有几个元素
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        print(__FUNCTION__)
        return self.data[section].count
    }
    
    // MARK: 生成Cell
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        print(__FUNCTION__)
        var cell = tableView.dequeueReusableCellWithIdentifier("cell")
        if cell == nil {
            cell = UITableViewCell(style: UITableViewCellStyle.Default, reuseIdentifier: "cell")
        }
        cell?.textLabel?.text = "\(self.data[indexPath.section][indexPath.row])"
        return cell!
    }
    
    // MARK: 组Header
    func tableView(tableView: UITableView, titleForHeaderInSection section: Int) -> String? {
        print(__FUNCTION__)
        return "\(section)--Header"
    }
    
    // MARK: 组Footer
    func tableView(tableView: UITableView, titleForFooterInSection section: Int) -> String? {
        print(__FUNCTION__)
        return "\(section)--Footer"
    }
    
    // MARK: 索引
    func sectionIndexTitlesForTableView(tableView: UITableView) -> [String]? {
        print(__FUNCTION__)
        var sectionTitles = [String]()
        for i in 0..<self.data.count {
            sectionTitles.append("\(i)")
        }
        return sectionTitles
    }
    
    // MARK: 索引对应的组
    func tableView(tableView: UITableView, sectionForSectionIndexTitle title: String, atIndex index: Int) -> Int {
        print(__FUNCTION__)
        return Int(title) ?? 0
    }
    
    // MARK: 能否编辑
    func tableView(tableView: UITableView, canEditRowAtIndexPath indexPath: NSIndexPath) -> Bool {
        print(__FUNCTION__)
        return true
    }
    
    // MARK: 增加和删除
    func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
        print(__FUNCTION__)
        if editingStyle == .Delete {
            // Delete the row from the data source
            self.data[indexPath.section].removeAtIndex(indexPath.row)
            tableView.deleteRowsAtIndexPaths([indexPath], withRowAnimation: .Fade)
        } else if editingStyle == .Insert {
            // Create a new instance of the appropriate class, insert it into the array, and add a new row to the table view
        }
    }
    
    // MARK: 能否移动
    func tableView(tableView: UITableView, canMoveRowAtIndexPath indexPath: NSIndexPath) -> Bool {
        print(__FUNCTION__)
        return true
    }
    
    // MARK: 移动cell
    func tableView(tableView: UITableView, moveRowAtIndexPath sourceIndexPath: NSIndexPath, toIndexPath destinationIndexPath: NSIndexPath) {
        print(__FUNCTION__)
        // 处理源数据
        let sourceData = self.data[sourceIndexPath.section][sourceIndexPath.row]
        self.data[sourceIndexPath.section].removeAtIndex(sourceIndexPath.row)
        self.data[destinationIndexPath.section].insert(sourceData, atIndex: destinationIndexPath.row)
    }
    
}
```

运行项目效果图如下所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015121401.png)

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[UITableViewDataSource Protocol Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-14 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog