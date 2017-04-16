Core Motion Framework使我们的应用接受运动事件，同时还支持我们处理加速计。对于有内置螺旋仪的设备，可以通过螺旋仪的数据反应当前设备的状态。这也就意味着，我们可以通过监听设备的状态给用户带来更好的应用体验。

# Classes

- NSObject
    - CMAltimeter 设备的海拔高度。
    - CMAttitude 设备的方向。
    - CMLogItem 运动事件的基类。
        - CMAccelerometerData 记录加速计数据。
            - CMRecordedAccelerometerData 代表一条加速计记录。
        - CMAltitudeData 记录相对高度变化。
        - CMDeviceMotion 设备方向、旋转速度和加速度。
        - CMGyroData 螺旋仪数据。
        - CMMagnetometerData 磁场计数据。
        - CMMotionActivity 当前用户所处状态，如在车里或走路。
    - CMMotionActivityManager 获取设备运动状态，如在车里或走路。
    - CMMotionManager 运动服务管理器。
    - CMPedometer 获取徒步行走数据。（IOS8启用，CMStepCounter的替代类）
    - CMPedometerData 记录徒步数据。
    - CMSensorDataList 枚举输出CMRecordedAccelerometerData数据。
    - CMSensorRecorder 和传感器交互，检索和存储运动数据。应用进入不活跃状态时，仍然记录数据。
    - CMStepCounter 记录徒步数据。（IOS7启用，IOS8废弃）

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