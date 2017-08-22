```swift
//
//  CMMotionActivity+Extension.swift
//  YJCoreMotion
//
//  Created by yangjun on 16/1/27.
//  Copyright © 2016年 阳君. All rights reserved.
//

import Foundation
import CoreMotion

@available(iOS 8.0, *)
public enum YJMotionActivity {
    /// there is no estimate of the current state.  This can happen if the device was turned off.
    case unknown
    /// the device is not moving.
    case stationary
    /// the device is on a walking person.
    case walking
    /// the device is on a running person.
    case running
    /// the device is in a vehicle.
    case automotive
    /// the device is on a bicycle.
    case cycling
}

@available(iOS 8.0, *)
public extension CMMotionActivity {
    
    /// 获取用户设备当前所处环境
    ///
    /// - returns: YJMotionActivity
    func motionActivity() -> YJMotionActivity {
        if self.stationary {
            return YJMotionActivity.stationary
        } else if self.walking {
            return YJMotionActivity.walking
        } else if self.running {
            return YJMotionActivity.running
        } else if self.automotive {
            return YJMotionActivity.automotive
        }else if self.cycling {
            return YJMotionActivity.cycling
        }
        return YJMotionActivity.unknown
    }
    
}
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Core Motion Framework Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CoreMotion_Reference/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-27 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog