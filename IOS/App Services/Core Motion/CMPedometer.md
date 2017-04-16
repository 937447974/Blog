CMPedometer是徒步计数器，用于记录用户走过的步数。

# 1 Determining Pedometer Availability

```swift
// 能否获取徒步数
public class func isStepCountingAvailable() -> Bool
    
// 能否获取距离
public class func isDistanceAvailable() -> Bool
    
// 能否获取楼层
public class func isFloorCountingAvailable() -> Bool
    
// 能否获取步速
@available(iOS 9.0, *)
public class func isPaceAvailable() -> Bool
    
// 能否获取节奏
@available(iOS 9.0, *)
public class func isCadenceAvailable() -> Bool
```

# 2 Generating Live Pedometer Data

```swift 
// 开始获取CMPedometerData数据
public func startPedometerUpdatesFromDate(start: NSDate, withHandler handler: CMPedometerHandler)
    
// 停止步数计获取数据
public func stopPedometerUpdates()
```

# 3 Fetching Historical Pedometer Data

```swift
// 获取某个时间段的徒步数据
public func queryPedometerDataFromDate(start: NSDate, toDate end: NSDate, withHandler handler: CMPedometerHandler)
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Core Motion Framework Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CoreMotion_Reference/index.html)

[CMPedometer Class Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CMPedometer_class/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-27 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog