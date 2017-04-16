Local Authentication framework使用指定的安全策略为用户提供身份验证。

如通过Touch ID验证可使用如下代码：

```objc
LAContext *myContext = [[LAContext alloc] init];
NSError *authError = nil;
NSString *myLocalizedReasonString = <#String explaining why app needs authentication#>;
 
if ([myContext canEvaluatePolicy:LAPolicyDeviceOwnerAuthenticationWithBiometrics error:&authError]) {
    [myContext evaluatePolicy:LAPolicyDeviceOwnerAuthenticationWithBiometrics
                  localizedReason:myLocalizedReasonString
                            reply:^(BOOL success, NSError *error) {
            if (success) {
                // User authenticated successfully, take appropriate action
            } else {
                // User did not authenticate successfully, look at error and take appropriate action
            }
        }];
} else {
    // Could not evaluate policy; look at authError and present an appropriate message to user
}
```

&#160;

----------

# Appendix

## Related Documentation

[Local Authentication Framework Reference](https://developer.apple.com/library/ios/documentation/LocalAuthentication/Reference/LocalAuthentication_Framework/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-08-03 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974