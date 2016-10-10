在Swift开发工程中，我们会封装一些常用的工具类，这里我封装了YJUtils。

```swift
//
//  YJUtils.swift
//  Utils
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/14.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// user interface
public struct YJUtilUserInterfaceIdiom {
    /// The user interface should be designed for iPhone and iPod touch.
    static let isPhone = UIDevice.currentDevice().userInterfaceIdiom == UIUserInterfaceIdiom.Phone
    /// The user interface should be designed for iPad.
    static let isPad = UIDevice.currentDevice().userInterfaceIdiom == UIUserInterfaceIdiom.Pad
    /// The user interface should be designed for Apple TV.
    static let isAppleTv = UIDevice.currentDevice().userInterfaceIdiom == UIUserInterfaceIdiom.TV
    
}


/// 屏幕尺寸
public struct YJUtilScreenSize {
    
    /// 屏幕宽
    static let screenWidth = UIScreen.mainScreen().bounds.size.width
    /// 屏幕高
    static let screenHeight = UIScreen.mainScreen().bounds.size.height
    /// 屏幕最大长度
    static let screenMaxLength = max(YJUtilScreenSize.screenWidth, YJUtilScreenSize.screenHeight)
    /// 屏幕最小长度
    static let screenMinLength = min(YJUtilScreenSize.screenMaxLength, YJUtilScreenSize.screenHeight)
    
}


/// 机型
public struct YJUtilDeviceType {
    
    /// IPhone4
    static let isIPhone4 = YJUtilUserInterfaceIdiom.isPhone && YJUtilScreenSize.screenMaxLength == 480.0
    /// IPhone5
    static let isIPhone5 = YJUtilUserInterfaceIdiom.isPhone && YJUtilScreenSize.screenMaxLength == 568.0
    /// IPhone6
    static let isIPhone6 = YJUtilUserInterfaceIdiom.isPhone && YJUtilScreenSize.screenMaxLength == 667.0
    /// IPhone6P
    static let isIPhone6P = YJUtilUserInterfaceIdiom.isPhone && YJUtilScreenSize.screenMaxLength == 736.0
    
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
| 2015-12-14 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog