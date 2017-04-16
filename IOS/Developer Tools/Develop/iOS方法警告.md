当苹果SDK升级时废弃某些方法，就需要方法警告。

外部调用方法警告

```objc
__deprecated_msg("方法废弃，请使用...替换")
```

当前方法警告

```objc
__attribute__((visibility("警告")));
```

如

```objc
- (void)test __deprecated_msg("方法废弃，请使用...替换") __attribute__((visibility("方法废弃")));
```

&#160;

----------

# Appendix

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-05-25 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974