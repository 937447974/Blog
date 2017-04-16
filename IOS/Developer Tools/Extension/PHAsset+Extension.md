```swift
//
//  PHAsset+Extension.swift
//  Photo
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/4.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit
import Photos

/// PHAsset扩展
public extension PHAsset {
    
    // MARK: - 删除图片
    /// 删除图片
    ///
    /// - parameter assetCollection : PHAssetCollection?
    /// - parameter assetCollection : 执行回调
    ///
    /// - returns: void
    func deleteWithPHAssetCollection(assetCollection: PHAssetCollection?, completionHandler: PHPhotoLibraryCompletionHandlerBlock = PHPhotoLibraryCompletionHandler) {
        let changeBlock: dispatch_block_t = {
            if assetCollection == nil { // 直接删除
                PHAssetChangeRequest.deleteAssets([self])
            } else if let changeRequest = PHAssetCollectionChangeRequest(forAssetCollection: assetCollection!) {
                // 从PHAssetCollection中删除
                changeRequest.removeAssets([self])
            }
        }
        PHPhotoLibrary.sharedPhotoLibrary().performChanges(changeBlock, completionHandler: completionHandler)
    }
    
    // MARK: - 收藏图片
    /// 收藏图片
    ///
    /// - parameter favorite : 是否收藏
    /// - parameter completionHandler : 执行完毕回调
    ///
    /// - returns: void
    func setFavorite(favorite: Bool, completionHandler: PHPhotoLibraryCompletionHandlerBlock = PHPhotoLibraryCompletionHandler) {
        let changeBlock: dispatch_block_t = {
            let request = PHAssetChangeRequest(forAsset: self)
            request.favorite = favorite
        }
        PHPhotoLibrary.sharedPhotoLibrary().performChanges(changeBlock, completionHandler: completionHandler)
    }
    
}
```

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-04 | 博文完成 |
| 2016-01-04 | 方法中增加闭包PHPhotoLibraryCompletionHandlerBlock |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog