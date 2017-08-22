**1 [MCBrowserViewController](#1)**

1. [Initializing a Browser View Controller](#1.1)
2. [Getting and Setting the Maximum and Minimum Number of Peers](#1.2)

**2 [MCBrowserViewControllerDelegate](#2)**

1. [Peer Notifications](#2.1)
2. [User Action Notifications](#2.2)

----

# <a id="1">1 MCBrowserViewController

MCBrowserViewController用于帮助用户发现附近的设备，并邀请加入会话。MCBrowserViewController是UIViewController的子类，也就意味着你可以用常用的方法完成跳转。

## <a id="1.1">1.1 Initializing a Browser View Controller

```swift
/// 初始化MCBrowserViewController
///
/// - parameter serviceType : 通话类型
/// - parameter session : 会话session
///
/// - returns: MCBrowserViewController
public convenience init(serviceType: String, session: MCSession)

/// 初始化MCBrowserViewController
///
/// - parameter browser : 附近的连接
/// - parameter session : 会话session
///
/// - returns: MCBrowserViewController
public init(browser: MCNearbyServiceBrowser, session: MCSession)
    
/// MCBrowserViewControllerDelegate代理
weak public var delegate: MCBrowserViewControllerDelegate?
/// 附近的Service
public var browser: MCNearbyServiceBrowser? { get }
/// 当前会话
public var session: MCSession { get }
```

## <a id="1.2">1.2 Getting and Setting the Maximum and Minimum Number of Peers

```swift
/// 最小连接数
public var minimumNumberOfPeers: Int
/// 最大连接数
public var maximumNumberOfPeers: Int
```

# <a id="2">2 MCBrowserViewControllerDelegate

## <a id="2.1">2.1 Peer Notifications

```swift   
/// 是否向附近的设备发出连接请求
@available(iOS 7.0, *)
optional public func browserViewController(browserViewController: MCBrowserViewController, shouldPresentNearbyPeer peerID: MCPeerID, withDiscoveryInfo info: [String : String]?) -> Bool
```

## <a id="2.2">2.2 User Action Notifications

```swift
/// 完成连接
@available(iOS 7.0, *)
public func browserViewControllerDidFinish(browserViewController: MCBrowserViewController)
    
/// 取消连接
@available(iOS 7.0, *)
public func browserViewControllerWasCancelled(browserViewController: MCBrowserViewController)
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Multipeer Connectivity Framework Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MultipeerConnectivityFramework/index.html)

[MCBrowserViewController Class Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCBrowserViewController_class/index.html)

[MCBrowserViewControllerDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCBrowserViewControllerDelegate/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-22 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog