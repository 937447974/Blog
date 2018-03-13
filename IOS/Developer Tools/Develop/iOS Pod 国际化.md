在 pod 库中支持国际化和图片显示。

项目结构

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2018031301.png)

podspec 修改

```
Pod::Spec.new do |s|

    s.resource_bundles = {
       'Test' => ['Pod/Assets/**/*']
    }

end
```

图片显示 cancel@2x.png

```objc
[UIImage imageNamed:@"Test.bundle/cancel"]
```

显示 `测试` 的英文国际化。

```objc
NSString *path = [@"Test.bundle" pathForResource:@"en" ofType:@"lproj"];
NSBundle *bundle = [NSBundle bundleWithPath:path];
NSString *localizedString = [bundle localizedStringForKey:@"测试" value:@"测试" table:@"Localizable"];
```

&#160;

----------

# Appendix

## Copyright

CSDN：[http://blog.csdn.net/y550918116j](http://blog.csdn.net/y550918116j)

GitHub：[https://github.com/937447974](https://github.com/937447974)