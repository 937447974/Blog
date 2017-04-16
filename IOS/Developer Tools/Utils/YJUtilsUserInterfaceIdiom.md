```swift
//
//  YJUtilsUserInterfaceIdiom.swift
//  Utils
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/14.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit

/// user interface
public struct YJUtilsUserInterfaceIdiom {
    
    /// The user interface should be designed for iPhone and iPod touch.
    static let isPhone = UIDevice.currentDevice().userInterfaceIdiom == UIUserInterfaceIdiom.Phone
    /// The user interface should be designed for iPad.
    static let isPad = UIDevice.currentDevice().userInterfaceIdiom == UIUserInterfaceIdiom.Pad
    /// The user interface should be designed for Apple TV.
    static let isAppleTV = UIDevice.currentDevice().userInterfaceIdiom == UIUserInterfaceIdiom.TV
    
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