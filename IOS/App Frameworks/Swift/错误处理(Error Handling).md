错误处理是在程序中响应错误和处理错误恢复程序正常运行的过程。Swift提供了抛出、捕获、传播和操作可恢复的过程。

有的操作不是每次都能执行成功的，而可选链是用来判断有没有值，但操作失败时，无法知道失败的原因。错误处理就是帮助你了解失败的原因，方便你做出相应的反应。

比如我们在网络传输时，会遇到各种各样的突发情况导致数据传输失败。这时，通过错误处理机制，就能有效的提示用户当前是因为那种错误导致网络出错。
&#160;

# 声明错误

声明错误用到了枚举，其中枚举实现ErrorType协议。这里我们使用枚举声明一个简单的错误。

```Swift
/// 错误枚举,需ErrorType协议
enum ErrorEnum: ErrorType {
    case Default // 普通错误
    case Other(tag: Int) // 高级错误，可携带数据
}        
```

这里面有两个错误Default和Other，其中Other可携带数据。
&#160;

# 抛出错误

我们声明了错误后，就在类中抛出错误，抛出错误只需要使用throw。

```Swift
throw ErrorEnum.Other(tag: 5)
```

区别常见的方法，能够抛出错误的方法体，需要添加throw。

```Swift
// 可抛出错误的方法体
func canThrowErrors() throws -> String
// 常见方法体
func cannotThrowErrors() -> String
```

这里我们创建一个类SomeClass，在类SomeClass中有一个能抛错的方法体。

```Swift
class SomeClass {
    
    func canThrowErrors(str: String) throws {
        // 当str不为Default时，输出错误
        guard str == "Default" else {
            throw ErrorEnum.Default
        }
        // 当str部位Other时输出错误
        guard str == "Other" else {
            throw ErrorEnum.Other(tag: 5)
        }
    }
    
}
```

这里用到了guard校验，然后抛出错误。如果你不知道guard语法，可查阅我的博文《[Swift控制流](http://blog.csdn.net/y550918116j/article/details/49428647)》。
&#160;

# 处理错误

在Swift捕获错误使用do-catch语法，如下所示：

![错误处理](http://img.blog.csdn.net/20151101155526346)

如图所示，你要执行的代码放在do的括号内，执行可抛出错误的代码使用try；catch就是捕获的意思，会执行每一个catch，匹配成功后，则执行内部代码，和switch中的case很像；where代表条件匹配，在错误匹配成功后，还要匹配where后的语句才会执行内部代码。如下所示：

```Swift
do {
    try sClass.canThrowErrors("Default")
    try sClass.canThrowErrors("Other")
} catch ErrorEnum.Default {
    print("默认错误")
} catch ErrorEnum.Other(let tag) where tag == 5 {
    print("错误代码：\(tag)")
} catch ErrorEnum.Other(let tag) {
    print("其他错误：\(tag)")
} catch {
    // 在捕获中，隐式携带error错误。
    print("未知错误：\(error)")
}
```

其中第一个try抛错后，则不会执行第二个try。

如果你不想写这么繁琐，Swift也支持只写一行代码，使用'!'。如：

```Swift
try! sClass.canThrowErrors("Default")
```

> 请慎重使用'!'，容易引起程序闪退。

&#160;

----------

# 其他

## 参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-1 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Error Handling总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j