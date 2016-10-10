```swift
//
//  YJUtilsDeviceType.swift
//  Utils
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/14.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit

/// 机型
public struct YJUtilsDeviceType {
    
    /// IPhone4
    static let isIPhone4 = YJUtilsUserInterfaceIdiom.isPhone && YJUtilsScreenSize.screenMaxLength == 480.0
    /// IPhone5
    static let isIPhone5 = YJUtilsUserInterfaceIdiom.isPhone && YJUtilsScreenSize.screenMaxLength == 568.0
    /// IPhone6
    static let isIPhone6 = YJUtilsUserInterfaceIdiom.isPhone && YJUtilsScreenSize.screenMaxLength == 667.0
    /// IPhone6P
    static let isIPhone6P = YJUtilsUserInterfaceIdiom.isPhone && YJUtilsScreenSize.screenMaxLength == 736.0
    
}
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-30 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog