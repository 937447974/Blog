**1 [MCNearbyServiceAdvertiser](#1)**

1. [Configuring and Initialization](#1.1)
2. [Starting and Stopping Advertisement](#1.2)

**2 [MCNearbyServiceAdvertiserDelegate](#2)**

1. [Error Handling Delegate Methods](#2.1)
2. [Invitation Handling Delegate Methods](#2.2)

----

#<a id="1">1 MCNearbyServiceAdvertiser

MCNearbyServiceAdvertiser通知附近的设备发出邀请。这个类会有回调，告知有用户要与您的设备连接，然后可以自定义提示框，以及自定义连接处理。

##<a id="1.1">1.1 Configuring and Initialization

```swift
/// 初始化MCNearbyServiceAdvertiser
public init(peer myPeerID: MCPeerID, discoveryInfo info: [String : String]?, serviceType: String)
/// MCNearbyServiceAdvertiserDelegate代理
weak public var delegate: MCNearbyServiceAdvertiserDelegate?
/// 当前MCPeerID
public var myPeerID: MCPeerID { get }
/// 携带信息
public var discoveryInfo: [String : String]? { get }
/// service类型
public var serviceType: String { get }
```

##<a id="1.2">1.2 Starting and Stopping Advertisement

```swift
/// 发出广播
public func startAdvertisingPeer()

/// 结束广播
public func stopAdvertisingPeer()
```

#<a id="2">2 MCNearbyServiceAdvertiserDelegate

##<a id="2.1">2.1 Error Handling Delegate Methods

```swift
/// 广播未运行的错误
@available(iOS 7.0, *)
optional public func advertiser(advertiser: MCNearbyServiceAdvertiser, didNotStartAdvertisingPeer error: NSError)
```

##<a id="2.2">2.2 Invitation Handling Delegate Methods

```swift
/// 附近的设备发出连接请求
@available(iOS 7.0, *)
public func advertiser(advertiser: MCNearbyServiceAdvertiser, didReceiveInvitationFromPeer peerID: MCPeerID, withContext context: NSData?, invitationHandler: (Bool, MCSession) -> Void)
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Multipeer Connectivity Framework Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MultipeerConnectivityFramework/index.html)

[MCNearbyServiceAdvertiser Class Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCNearbyServiceAdvertiserClassRef/index.html)

[MCNearbyServiceAdvertiserDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCNearbyServiceAdvertiserDelegateProtocolRef/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-22 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog