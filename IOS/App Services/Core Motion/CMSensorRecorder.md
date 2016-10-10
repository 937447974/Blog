CMSensorRecorder是和传感器交互，检索和存储运动数据。应用进入不活跃状态时，仍然记录数据。

#1 Checking the Availability of Sensor Recording

```swift
// 加速计数据能否记录数据
public class func isAccelerometerRecordingAvailable() -> Bool
    
// 是否授权读取数据
public class func isAuthorizedForRecording() -> Bool
```

#2 Retrieving Past Accelerometer Data

```swift
// 根据标示符获取数据
public func accelerometerDataSince(identifier: UInt64) -> CMSensorDataList?
    
// 返回某个时间段的数据
public func accelerometerDataFrom(fromDate: NSDate, to toDate: NSDate) -> CMSensorDataList?
    
// 开始记录数据，可长达12小时，频率50hz
public func recordAccelerometerFor(duration: NSTimeInterval)
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Core Motion Framework Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CoreMotion_Reference/index.html)

[CMSensorRecorder Class Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CMSensorRecorder_class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-27 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog