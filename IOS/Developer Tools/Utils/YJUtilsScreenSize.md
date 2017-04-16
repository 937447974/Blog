```swift
//
//  YJUtilsScreenSize.swift
//  Utils
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/14.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit

/// 屏幕尺寸
public struct YJUtilsScreenSize {
    
    /// 屏幕宽
    static let screenWidth = UIScreen.mainScreen().bounds.size.width
    /// 屏幕高
    static let screenHeight = UIScreen.mainScreen().bounds.size.height
    /// 屏幕最大长度
    static let screenMaxLength = max(YJUtilsScreenSize.screenWidth, YJUtilsScreenSize.screenHeight)
    /// 屏幕最小长度
    static let screenMinLength = min(YJUtilsScreenSize.screenWidth, YJUtilsScreenSize.screenHeight)
    
}
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-30 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog