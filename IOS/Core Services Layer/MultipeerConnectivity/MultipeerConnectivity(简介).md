MultipeerConnectivity支持点对点连接和发现附近的设备。当无法连接互联网时，MultipeerConnectivity也能帮助我们在相近的设备间传输数据，如消息、流数据或文件。

#Using the Framework

使用这个库我们会经历两个阶段，发现阶段和会话阶段。

#Classes

- NSObject
    - MCAdvertiserAssistant 向用户显示传入邀请和处理用户的响应。
    - MCNearbyServiceAdvertiser 发出广播通知，供附件的设备发现并邀请。
    - MCNearbyServiceBrowser 搜索发现附近的设备，并邀请加入会话。
    - MCPeerID MCSession会话中的设备。
    - MCSession 所有连接设备的共有会话。

- UIViewController
    - MCBrowserViewController UI搜索显示附近的设备并允许用户邀请搜索的设备加入会话。

#Protocols

- MCAdvertiserAssistantDelegate MCAdvertiserAssistant代理，处理广播相关事件。
- MCNearbyServiceAdvertiserDelegate MCNearbyServiceAdvertiser代理，处理发出广播的事件。
- MCNearbyServiceBrowserDelegate MCNearbyServiceBrowser代理，处理发现设备的事件。
- MCSessionDelegate MCSession代理，处理与会话相关的事件。
- MCBrowserViewControllerDelegate MCBrowserViewController代理，处理取消连接、完成连接和发出连接请求的事件。

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Multipeer Connectivity Framework Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MultipeerConnectivityFramework/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-21 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog