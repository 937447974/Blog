```swift
//
//  UICollectionView+Extension.swift
//  UI
//
//  Created by yangjun on 15/12/20.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// UICollectionView扩展
public extension UICollectionView {

    // MARK: - 移动Item
    /// 允许移动Item，默认不允许
    ///
    /// - returns: void
    func allowsMoveItem() {
        // 长点击事件，做移动cell操作
        let longPressGesture = UILongPressGestureRecognizer(target: self, action: "handleLongGestureMove:")
        self.addGestureRecognizer(longPressGesture)
    }
    
    // MARK: 长点击事件
    func handleLongGestureMove(gesture: UILongPressGestureRecognizer) {
        switch(gesture.state) {
        case UIGestureRecognizerState.Began:// 开始移动
            let point = gesture.locationInView(gesture.view!)
            if let selectedIndexPath = self.indexPathForItemAtPoint(point) {
                self.beginInteractiveMovementForItemAtIndexPath(selectedIndexPath) // 开始移动
            }
        case UIGestureRecognizerState.Changed:
            // 移动中
            let point = gesture.locationInView(gesture.view!)
            self.updateInteractiveMovementTargetPosition(point)
        case UIGestureRecognizerState.Ended:
            // 结束移动
            self.endInteractiveMovement()
        default:
            // 取消移动
            self.cancelInteractiveMovement()
        }
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