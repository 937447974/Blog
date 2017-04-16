AdSupport是广告支持框架，提供访问用户的广告标示符号，以及判断用户是否限制了广告跟踪。

# Classes

- NSObject
    - ASIdentifierManager 记录广告标示符和判断用户是否限制了广告跟踪。

## ASIdentifierManager

### Getting the Advertising Identifier

```swift
// 共享ASIdentifierManager
public class func sharedManager() -> ASIdentifierManager!
    
// 广告标示符
public var advertisingIdentifier: NSUUID! { get }
// 用户是否限制了广告跟踪
public var advertisingTrackingEnabled: Bool { get }
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[Ad Support Framework Reference](https://developer.apple.com/library/ios/documentation/DeviceInformation/Reference/AdSupport_Framework/index.html)

[ASIdentifierManager Class Reference](https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog