使用CTCallCenter可获取当前电话和对方电话的通话状态。

# Responding to Cellular Call Events

```swift
// 获取接入电话
@available(iOS 4.0, *)
public var currentCalls: Set<CTCall>? { get }

// block监听通话状态
@available(iOS 4.0, *)
public var callEventHandler: ((CTCall) -> Void)?
```

&#160;

----

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Core Telephony Framework Reference](https://developer.apple.com/library/ios/documentation/CoreSpotlight/Reference/CoreSpotlight_Framework/index.html)

[CTCallCenter Class Reference](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Reference/CTCallCenter/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-30 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog