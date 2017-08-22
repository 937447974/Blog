```
//
//  YJUtilsDirectory.swift
//  YJFoundation
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by admin on 16/3/20.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit

/// app应用目录
public struct YJUtilsDirectory {
    
    // MARK: - path路径
    /// HomeDirectoryPath
    static let homePath = NSHomeDirectory()
    /// DocumentDirectoryPath
    static let documentPath = NSSearchPathForDirectoriesInDomains(NSSearchPathDirectory.DocumentDirectory, NSSearchPathDomainMask.UserDomainMask, true).first!
    /// LibraryDirectoryPath
    static let libraryPath = NSSearchPathForDirectoriesInDomains(.LibraryDirectory, .UserDomainMask, true).first!
    /// CachesDirectoryPath
    static let cachesPath = NSSearchPathForDirectoriesInDomains(.CachesDirectory, .UserDomainMask, true).first!
    /// TemporaryDirectoryPath
    static let tempPath = NSTemporaryDirectory()
    
    // MARK: - url 路径
    /// HomeDirectoryURL
    static let homeURL = NSURL(fileURLWithPath: YJUtilsDirectory.homePath)
    /// DocumentDirectoryURL
    static let documentURL = NSURL(fileURLWithPath: YJUtilsDirectory.homePath)
    /// LibraryDirectoryURL
    static let libraryURL = NSURL(fileURLWithPath: YJUtilsDirectory.libraryPath)
    /// CachesDirectoryURL
    static let cachesURL = NSURL(fileURLWithPath: YJUtilsDirectory.cachesPath)
    /// TemporaryDirectoryURL
    static let tempURL = NSURL(fileURLWithPath: YJUtilsDirectory.tempPath)
    
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
| 2016-03-20 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog