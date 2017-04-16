**1 [MCNearbyServiceBrowser](#1)**

1. [Initializing the Browser](#1.1)
2. [Browsing for Peers](#1.2)
3. [Inviting Peers](#1.3)

**2 [MCSessionDelegate](#2)**

1. [Error Handling Delegate Methods](#2.1)
2. [Peer Discovery Delegate Methods](#2.2)

----

# <a id="1">1 MCNearbyServiceBrowser

MCNearbyServiceBrowser主要用于发现附近的设备。

## <a id="1.1">1.1 Initializing the Browser

```swift
/// 初始化MCNearbyServiceBrowser
public init(peer myPeerID: MCPeerID, serviceType: String)
    
/// MCNearbyServiceBrowserDelegate代理
weak public var delegate: MCNearbyServiceBrowserDelegate?
/// 当前MCPeerID
public var myPeerID: MCPeerID { get }
/// service类型
public var serviceType: String { get }
```

## <a id="1.2">1.2 Browsing for Peers

```swift
/// 开始搜索设备
public func startBrowsingForPeers()
/// 结束搜索设备
public func stopBrowsingForPeers()
```

## <a id="1.3">1.3 Inviting Peers

```swift
/// 邀请设备加入会话
public func invitePeer(peerID: MCPeerID, toSession session: MCSession, withContext context: NSData?, timeout: NSTimeInterval)
```

# <a id="2">2 MCSessionDelegate

## <a id="2.1">2.1 Error Handling Delegate Methods

```swift
// 开启搜索附近设备失败
@available(iOS 7.0, *)
optional public func browser(browser: MCNearbyServiceBrowser, didNotStartBrowsingForPeers error: NSError)
```

## <a id="2.2">2.2 Peer Discovery Delegate Methods

```swift
// 发现附近的MCPeerID
@available(iOS 7.0, *)
public func browser(browser: MCNearbyServiceBrowser, foundPeer peerID: MCPeerID, withDiscoveryInfo info: [String : String]?)

// 某个MCPeerID消失了
@available(iOS 7.0, *)
public func browser(browser: MCNearbyServiceBrowser, lostPeer peerID: MCPeerID)
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Multipeer Connectivity Framework Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MultipeerConnectivityFramework/index.html)

[MCNearbyServiceBrowser Class Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCNearbyServiceBrowserClassRef/index.html)

[MCNearbyServiceBrowserDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCNearbyServiceBrowserDelegateRef/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-22 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog