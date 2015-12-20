```swift
//
//  UITableView+Extension.swift
//  UI
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/14.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

public extension UITableView {
    
    // MARK: - 去掉tableHeaderView
    /// 去掉tableHeaderView
    ///
    /// - returns: void
    func removeTableHeaderView() {
        self.tableHeaderView = UIView(frame:  CGRectMake(0, 0, YJUtilScreenSize.screenWidth, 0.1))
    }
    
    // MARK: - 去掉tableFooterView
    /// 去掉tableFooterView
    ///
    /// - returns: void
    func removeTableFooterView() {
        self.tableFooterView = UIView(frame:  CGRectMake(0, 0, YJUtilScreenSize.screenWidth, 0.1))
    }
    
}
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-20 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog