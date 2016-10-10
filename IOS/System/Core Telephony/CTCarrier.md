使用CTCarrier获取电话服务商的信息，如它的唯一标示，它是否允许VoIP的网络电话。

#Obtaining Information About the Cellular Service Provider

```swift
 // 供应商信息
@available(iOS 4.0, *)
public var carrierName: String? { get }

 // 移动电话的国家编码
@available(iOS 4.0, *)
public var mobileCountryCode: String? { get }
    
// 移动电话编码
@available(iOS 4.0, *)
public var mobileNetworkCode: String? { get }

// 移动电话服务商的IOS国家编码
@available(iOS 4.0, *)
public var isoCountryCode: String? { get }
    
// 是否允许VoIP通话
@available(iOS 4.0, *)
public var allowsVOIP: Bool { get }

```

&#160;

----

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Core Telephony Framework Reference](https://developer.apple.com/library/ios/documentation/CoreSpotlight/Reference/CoreSpotlight_Framework/index.html)

[CTCarrier Class Reference](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/CTCarrier/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-29 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog