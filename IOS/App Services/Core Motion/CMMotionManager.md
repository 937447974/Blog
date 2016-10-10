1. [Managing Accelerometer Updates](#1)
2. [Determining Whether the Accelerometer Is Active and Available](#2)
3. [Accessing Accelerometer Data](#3)
4. [Managing Gyroscope Updates](#4)
5. [Determining Whether the Gyroscope Is Active and Available](#5)
6. [Accessing Gyroscope Data](#6)
7. [Managing Magnetometer Updates](#7)
8. [Determining Whether the Magnetometer is Active and Available](#8)
9. [Accessing Magnetometer Data](#9)
10. [Managing the Device Movement Display](#10)
11. [Managing Device Motion Updates](#11)
12. [Accessing Attitude Reference Frames](#12)
13. [Determining Whether the Device Motion Hardware Is Active and Available](#13)
14. [Accessing Device Motion Data](#14)

----

CMMotionManager可以理解为是CoreMotion Framework的中央管理器，也可以理解为运动服务。这些服务提供了获取加速机数据、旋转数据和磁力数据等。

#<a id="1">1 Managing Accelerometer Updates

```swift
// 加速计更新间隔
public var accelerometerUpdateInterval: NSTimeInterval
    
// 开始获取加速计数据
public func startAccelerometerUpdates()
    
// 队列中获取加速计数据
public func startAccelerometerUpdatesToQueue(queue: NSOperationQueue, withHandler handler: CMAccelerometerHandler)
    
// 停止获取加速计数据
public func stopAccelerometerUpdates()
```

#<a id="2">2 Determining Whether the Accelerometer Is Active and Available
    
```swift
// 设备是否支持加速计
public var accelerometerAvailable: Bool { get }

// 加速计是否发生更新
public var accelerometerActive: Bool { get }
```

#<a id="3">3 Accessing Accelerometer Data
    
```swift
// 最新的加速计数据
public var accelerometerData: CMAccelerometerData? { get }
```

#<a id="4">4 Managing Gyroscope Updates
    
```swift
// 螺旋仪获取数据间隔
public var gyroUpdateInterval: NSTimeInterval
    
// 开始获取螺旋仪数据
public func startGyroUpdates()
    
// 队列中获取螺旋仪数据
public func startGyroUpdatesToQueue(queue: NSOperationQueue, withHandler handler: CMGyroHandler)
    
// 停止获取螺旋仪数据
public func stopGyroUpdates()
```

#<a id="5">5 Determining Whether the Gyroscope Is Active and Available
    
```swift
// 设备是否支持螺旋仪
public var gyroAvailable: Bool { get }

// 螺旋仪是否有更新
public var gyroActive: Bool { get }
```

#<a id="6">6 Accessing Gyroscope Data
    
```swift
// 最新螺旋仪数据
public var gyroData: CMGyroData? { get }
```

#<a id="7">7 Managing Magnetometer Updates
    
```swift
// 磁场计获取数据间隔
@available(iOS 5.0, *)
public var magnetometerUpdateInterval: NSTimeInterval
    
// 开始获取磁场计数据
@available(iOS 5.0, *)
public func startMagnetometerUpdates()
    
// 队列中获取磁场计数据
@available(iOS 5.0, *)
public func startMagnetometerUpdatesToQueue(queue: NSOperationQueue, withHandler handler: CMMagnetometerHandler)
    
// 停止获取磁场计数据
@available(iOS 5.0, *)
public func stopMagnetometerUpdates()
```

#<a id="8">8 Determining Whether the Magnetometer is Active and Available
    
```swift
// 设备是否支持磁场计
@available(iOS 5.0, *)
public var magnetometerAvailable: Bool { get }
    
// 磁场计是否有更新
@available(iOS 5.0, *)
public var magnetometerActive: Bool { get }
```

#<a id="9">9 Accessing Magnetometer Data
    
```swift
// 最新磁场计数据
@available(iOS 5.0, *)
public var magnetometerData: CMMagnetometerData? { get }
```

#<a id="10">10 Managing the Device Movement Display
    
```swift
// 是否显示设备运动
@available(iOS 5.0, *)
public var showsDeviceMovementDisplay: Bool
```

#<a id="11">11 Managing Device Motion Updates
    
```swift
// 获取运动数据更新间隔
public var deviceMotionUpdateInterval: NSTimeInterval

// 开始持续获取运动数据
public func startDeviceMotionUpdates()

// 队列获取CMDeviceMotion
public func startDeviceMotionUpdatesToQueue(queue: NSOperationQueue, withHandler handler: CMDeviceMotionHandler)

// 设置坐标系
@available(iOS 5.0, *)
public func startDeviceMotionUpdatesUsingReferenceFrame(referenceFrame: CMAttitudeReferenceFrame)

// 队列根据坐标系获取CMDeviceMotion
@available(iOS 5.0, *)
public func startDeviceMotionUpdatesUsingReferenceFrame(referenceFrame: CMAttitudeReferenceFrame, toQueue queue: NSOperationQueue, withHandler handler: CMDeviceMotionHandler)
    
// 停止获取运动数据
public func stopDeviceMotionUpdates()
```

#<a id="12">12 Accessing Attitude Reference Frames
    
```swift
// 获取设备参考系
@available(iOS 5.0, *)
public class func availableAttitudeReferenceFrames() -> CMAttitudeReferenceFrame

// 获取参考系
@available(iOS 5.0, *)
public var attitudeReferenceFrame: CMAttitudeReferenceFrame { get }
```

#<a id="13">13 Determining Whether the Device Motion Hardware Is Active and Available
    
```swift
// 是否支持获取运动数据
public var deviceMotionAvailable: Bool { get }
    

// CMDeviceMotion是否有更新
public var deviceMotionActive: Bool { get }
```

#<a id="14">14 Accessing Device Motion Data
    
```swift
// 获取设备运动数据
public var deviceMotion: CMDeviceMotion? { get }
```


&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Core Motion Framework Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CoreMotion_Reference/index.html)

[CMMotionManager Class Reference](https://developer.apple.com/library/ios/documentation/CoreMotion/Reference/CMMotionManager_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-27 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog