CTTelephonyNetworkInfo监听移动服务提供商的变化。如，开机状态更换SIM卡。

# Obtaining Information About the Cellular Service Provider

```swift
// 当前移动用户提供商
@available(iOS 4.0, *)
public var subscriberCellularProvider: CTCarrier? { get }

// 移动用户提供商发生变化
@available(iOS 4.0, *)
public var subscriberCellularProviderDidUpdateNotifier: ((CTCarrier) -> Void)?
    
// 设备注册的数据接入信息
@available(iOS 7.0, *)
public var currentRadioAccessTechnology: String? { get }
```

&#160;

----

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Core Telephony Framework Reference](https://developer.apple.com/library/ios/documentation/CoreSpotlight/Reference/CoreSpotlight_Framework/index.html)

[CTTelephonyNetworkInfo Class Reference](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/CTTelephonyNetworkInfo/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-29 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog