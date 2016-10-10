**1 [MCSession](#1)**

1. [Creating a Session](#1.1)
2. [Managing Peers Manually](#1.2)
3. [Sending Data and Resources](#1.3)
4. [Leaving a Session](#1.4)

**2 [MCSessionDelegate](#2)**

1. [MCSession Delegate Methods](#2.1)

----

#<a id="1">1 MCSession

MCSession用于保存MultipeerConnectivity框架通信过程中的会话，并发送相关数据。

##<a id="1.1">1.1 Creating a Session

```swift
/// 通过当前设备的MCPeerID初始化MCSession
public convenience init(peer myPeerID: MCPeerID)
    
/// 初始化MCSession，并提供安全证书
public init(peer myPeerID: MCPeerID, securityIdentity identity: [AnyObject]?, encryptionPreference: MCEncryptionPreference)

/// MCSessionDelegate代理
weak public var delegate: MCSessionDelegate?
/// 当前MCPeerID
public var myPeerID: MCPeerID { get }
/// 安全证书
public var securityIdentity: [AnyObject]? { get }
/// 连接是否加密
public var encryptionPreference: MCEncryptionPreference { get }
```

##<a id="1.2">1.2 Managing Peers Manually

```swift
/// 手动连接某台设备
public func connectPeer(peerID: MCPeerID, withNearbyConnectionData data: NSData)
    
/// 取消和某个设备的连接
public func cancelConnectPeer(peerID: MCPeerID)

/// 其他连接的MCPeerID
public var connectedPeers: [MCPeerID] { get }

/// 从远程的MCPeerID获取数据
public func nearbyConnectionDataForPeer(peerID: MCPeerID, withCompletionHandler completionHandler: (NSData, NSError?) -> Void)
```

##<a id="1.3">1.3 Sending Data and Resources

```swift
/// 发送NSData数据到其他设备
public func sendData(data: NSData, toPeers peerIDs: [MCPeerID], withMode mode: MCSessionSendDataMode) throws
    
/// 发送文件数据到其他设备
public func sendResourceAtURL(resourceURL: NSURL, withName resourceName: String, toPeer peerID: MCPeerID, withCompletionHandler completionHandler: ((NSError?) -> Void)?) -> NSProgress?
    
/// 以流的方式发送数据
public func startStreamWithName(streamName: String, toPeer peerID: MCPeerID) throws -> NSOutputStream
```

##<a id="1.4">1.4 Leaving a Session

```swift
/// 从会话中断开连接
public func disconnect()
```

#<a id="2">2 MCSessionDelegate

MCSessionDelegate用于监听处理MCSession的相关事件。

##<a id="2.1">2.1 MCSession Delegate Methods

```swift
// 附近设备的会话状态发生变化
@available(iOS 7.0, *)
public func session(session: MCSession, peer peerID: MCPeerID, didChangeState state: MCSessionState)
    
// 首次连接时的安全证书验证
@available(iOS 7.0, *)
optional public func session(session: MCSession, didReceiveCertificate certificate: [AnyObject]?, fromPeer peerID: MCPeerID, certificateHandler: (Bool) -> Void)
    
// 从附近的设备接受NSData数据
@available(iOS 7.0, *)
public func session(session: MCSession, didReceiveData data: NSData, fromPeer peerID: MCPeerID)
    
// 从附近的设备开始接受NSProgress数据
@available(iOS 7.0, *)
public func session(session: MCSession, didStartReceivingResourceWithName resourceName: String, fromPeer peerID: MCPeerID, withProgress progress: NSProgress)
    
// 从附近的设备结束接受NSProgress数据，并存储在一个本地地址
@available(iOS 7.0, *)
public func session(session: MCSession, didFinishReceivingResourceWithName resourceName: String, fromPeer peerID: MCPeerID, atURL localURL: NSURL, withError error: NSError?)
    
// 从附近的设备接受NSInputStream数据
@available(iOS 7.0, *)
public func session(session: MCSession, didReceiveStream stream: NSInputStream, withName streamName: String, fromPeer peerID: MCPeerID)
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Multipeer Connectivity Framework Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MultipeerConnectivityFramework/index.html)

[MCSession Class Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCSessionClassRef/index.html)

[MCSessionDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCSessionDelegateRef/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-22 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog