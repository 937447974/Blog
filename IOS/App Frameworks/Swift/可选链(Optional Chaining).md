了解过设计模式的人都知道责任链模式（如果你不知道什么是责任链模式，可以阅读我的博文《[23设计模式之责任链模式 (COR)](http://blog.csdn.net/y550918116j/article/details/48596903)》），在OC的手势响应链也是基于这种模式开发的。

责任链模式的核心可以理解为

```
if {

} else if {

} else {

}
```
如果每一次都这样写，代码就特别多，也不美观。

Swift考虑到这种情况设计了可选链，可选链的核心是两个操作符号:

- **?**：当'?'前有值时，执行'?'后面代码，为nil时不执行'?'后面的代码;如果是赋值的时候使用，则意味着这个常量、变量可能为有值或nil。
- **!**：不管'!'前有值还是nil，都执行‘!’后面的代码，你可以理解为强制调用；如果是赋值的时候使用，则意味着这个常量、变量一定有值。

> '!'要慎重使用，不然会引起程序崩溃，app闪退。

这里设计两个类：

```Swift
class Residence {
    var numberOfRooms = 1
}

class Person {
    // 可选属性，可能为nil或Residence类
    var residence: Residence?
}
```

在Person中有个引用类型的属性residence，这个属性可以是nil也可以是Residence类，这就是一个可选属性。

下面给大家介绍可选链的使用。

## 可选获得

```Swift
let john = Person()
john.residence = Residence()
    
// 可选获得
var roomCount = john.residence?.numberOfRooms
```

## 强制获得

如果你已经确定这个属性有值是，你可以使用‘!’强制获得。
```Swift
// 强制获得
roomCount = john.residence!.numberOfRooms
print(roomCount) // Optional(1) Optional代表可选
```

## if获得

在上面两种方式都比较麻烦，第一种方式，当你去使用的时候，你还需要去if判断;第二种方式，不是每一次都能强制调用成功的,当为nil时强制调用，会引起程序崩溃。处于这种情况，我们可以结合if使用更优雅的方式获得。

```Swift
// if获得
if let roomCount = john.residence?.numberOfRooms {
    print(roomCount) // 1
}
```

&#160;

----------

# 其他

## 参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-1 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Optional Chaining总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j