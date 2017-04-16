LAContext表示身份验证上下文。

身份验证上下文用来评估认证策略，允许应用程序要求用户使用个人信息。如Touch ID。

# Evaluating Authentication Policies

```swift
// 是否开启身份验证
@available(iOS 8.0, *)
public func canEvaluatePolicy(policy: LAPolicy, error: NSErrorPointer) -> Bool
// 身份验证是否通过
@available(iOS 8.0, *)
public func evaluatePolicy(policy: LAPolicy, localizedReason: String, reply: (Bool, NSError?) -> Void)
```

&#160;

----------

# Appendix

## Related Documentation

[LAContext Class Reference](https://developer.apple.com/library/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-08-03 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974