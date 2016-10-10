CMAltimeter是高度计，用于记录设备的高度变化。

#1 Determining Altitude Availability

```swift
// 能否获取高度
public class func isRelativeAltitudeAvailable() -> Bool
```

#2 Starting and Stopping Altitude Updates

```swift
// 开始获取高度
public func startRelativeAltitudeUpdatesToQueue(queue: NSOperationQueue, withHandler handler: CMAltitudeHandler)
    
// 停止获取高度
public func stopRelativeAltitudeUpdates()
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Core Motion Framework Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CoreMotion_Reference/index.html)

[CMAltimeter Class Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CMAltimeter_class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-27 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog