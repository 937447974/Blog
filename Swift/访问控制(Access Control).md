当你在Swift编写一个类时，默认其中任何属性，方法都能被外部访问的。有的时候我们不希望属性或方法被外部访问，希望私有化。

在swfit用于访问控制的有三个关键字。

- public：公共访问，允许在任何源文件中使用其定义模块。如你使用XCTest测试某个类时，就需要在类前添加public。
- internal：swift默认访问控制，允许在项目内访问。
- private：私人访问，只能在当前类中访问。如果是在class前添加，则只能是当前文件访问。

举例说明：

```swift
public class SomePublicClass {          // 明确 public class
    public var somePublicProperty = 0    // 明确 public class 成员
    var someInternalProperty = 0         // 默认 internal class 成员
    private func somePrivateMethod() {}  // 明确 private class 成员
}

class SomeInternalClass {               // 默认 internal class
    var someInternalProperty = 0         // 默认 internal class 成员
    private func somePrivateMethod() {}  // 明确 private class 成员
}

private class SomePrivateClass {        // 明确 private class
    var somePrivateProperty = 0          // 默认 private class 成员
    func somePrivateMethod() {}          // 默认 private class 成员
}
```

&#160;

----------

#其他

##参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-1 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Classes and Structures总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j