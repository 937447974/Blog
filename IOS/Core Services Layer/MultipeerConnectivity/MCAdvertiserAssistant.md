**1 [MCAdvertiserAssistant](#1)**

1. [Configuring and Initialization](#1.1)
2. [Starting and Stopping the Assistant](#1.2)

**2 [MCAdvertiserAssistantDelegate](#2)**

1. [Advertiser Assistant Delegate Methods](#2.1)

----

#<a id="1">1 MCAdvertiserAssistant

MCAdvertiserAssistant主要用户发出广播供附近的设备发现。

##<a id="1.1">1.1 Configuring and Initialization

```swift
/// 初始化MCAdvertiserAssistant
public init(serviceType: String, discoveryInfo info: [String : String]?, session: MCSession)
    
/// MCAdvertiserAssistantDelegate代理
weak public var delegate: MCAdvertiserAssistantDelegate?
/// 会话
public var session: MCSession { get }
/// 信息字典
public var discoveryInfo: [String : String]? { get }
/// 标示名称
public var serviceType: String { get }
```

##<a id="1.2">1.2 Starting and Stopping the Assistant

```swift
/// 开始广播
public func start()
    
/// 结束广播
public func stop()
```

#<a id="2">2 MCAdvertiserAssistantDelegate


##<a id="2.1">2.1 Advertiser Assistant Delegate Methods

```swift
/// 发出请求
@available(iOS 7.0, *)
optional public func advertiserAssistantWillPresentInvitation(advertiserAssistant: MCAdvertiserAssistant)
    
/// 结束请求
@available(iOS 7.0, *)
optional public func advertiserAssistantDidDismissInvitation(advertiserAssistant: MCAdvertiserAssistant)
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Multipeer Connectivity Framework Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MultipeerConnectivityFramework/index.html)

[MCAdvertiserAssistant Class Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCAdvertiserAssistant_class/index.html)

[MCAdvertiserAssistantDelegate Class Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCAdvertiserAssistantDelegate_class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-22 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog