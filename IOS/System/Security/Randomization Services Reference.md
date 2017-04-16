动态生成一个指定位数的随机数。

```
// 生成随机数
@available(iOS 2.0, *)
public func SecRandomCopyBytes(rnd: SecRandomRef, _ count: Int, _ bytes: UnsafeMutablePointer<UInt8>) -> Int32
```

&#160;

----------

# Appendix

## Related Documentation

[Security Framework Reference](https://developer.apple.com/library/ios/documentation/Security/Reference/SecurityFrameworkReference/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-07-29 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974