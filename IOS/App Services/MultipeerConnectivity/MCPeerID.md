MCPeerID代表在点对点通信过程中的设备。

# Peer Methods

```swift
/// 初始化MCPeerID
///
/// - parameter myDisplayName : 昵称，UTF-8编码不能超过63bytes
///
/// - returns: void
public init(displayName myDisplayName: String)
    
/// 获取显示名
public var displayName: String { get }
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Multipeer Connectivity Framework Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MultipeerConnectivityFramework/index.html)

[MCPeerID Class Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MCPeerID_class/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-21 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog