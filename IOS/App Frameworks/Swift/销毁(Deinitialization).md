在Swift中，也支持ARC机制，也就是内存自动回收机制。

在上一篇博文我们讲了《[Swift初始化(Initialization)](http://blog.csdn.net/y550918116j/article/details/49535219)》，既然有了初始化的方法，Swift也提供了销毁的通知方法。

> deinit只能在引用类型中使用，也就是只能在类中使用。

```Swift
deinit {
    // perform the deinitialization
}
```

下面就用一个例子给大家介绍销毁是怎么工作的。

```Swift
class SomeClass {
    
    // MARK: 类销毁时，通知此方法
    deinit {
        print("销毁")
    }
    
}
    
var sClass:SomeClass? = SomeClass()
sClass = nil // print "销毁"
```

我们设置变量sClass为可选的SomeClass类，当我们将sClass设为nil时，当然你也可以不执行这行代码，swift会自动回收sClass分配的内存空间。

这里我们人为的调用`sClass = nil`后，Swift开始销毁sClass，销毁完毕通知SomeClass类中的deinit方法体，最后完成内存回收。

&#160;

----------

# 其他

## 参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-10-31 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Deinitialization总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j