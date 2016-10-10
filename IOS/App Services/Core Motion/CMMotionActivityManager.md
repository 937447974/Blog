CMMotionActivityManager用于获取用户当前所处的状态，如在自行车、车里或徒步行走等。

#1 Determining Activity Availability

```swift
// 能否获取活动状态
public class func isActivityAvailable() -> Bool
```

#2 Starting and Stopping Activity Updates

```swift
// 队列中获取活动状态
public func startActivityUpdatesToQueue(queue: NSOperationQueue, withHandler handler: CMMotionActivityHandler)
    
/// 停止获取活动状态
public func stopActivityUpdates()
```

#3 Getting Historical Activity Data

```swift
// 获取某个时间段的活动状态
public func queryActivityStartingFromDate(start: NSDate, toDate end: NSDate, toQueue queue: NSOperationQueue, withHandler handler: CMMotionActivityQueryHandler)
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Core Motion Framework Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CoreMotion_Reference/index.html)

[CMMotionActivityManager Class Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CMMotionActivityManager_class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-27 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog