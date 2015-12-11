协议主要为一个特定的任务和功能定义一个方法、属性和其他要求，你也可以理解协议就是一种要遵循的规范。

学过设计模式的，都知道工厂模式，如果你不知道可以查阅我的博文《[23设计模式之工厂方法(FactoryMethod)](http://blog.csdn.net/y550918116j/article/details/48596527)》，工厂模式就是一种协议的体现。在Java中，是用接口定义协议的；在OC中，主要用于代理。

除了已有的协议，你还可以像扩展类一样扩展协议。这些扩展的协议可以实现也可以直接使用。

#语法

协议语法使用了关键字protocol，和其他类型不同的是，它是规范的指定者，无须去实现。下面是一个空得协议。

```swift
protocol SomeProtocol {
    // protocol definition goes here
}
```

协议可以被结构体继承，

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // structure definition goes here
}
```

协议也可以被类继承，当然这是它最主要的功能。

```Swift
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // class definition goes here
}
```

##协议

这里我们设计一个协议，并实现一个简单方法。我们在这里使用了一个简单方法。

```Swift
protocol YJSomeProtocol {
    
    func test()
     
}
```

编写一个类去实现这个协议。

```swift
class YJSomeClass: YJSomeProtocol {

    func test() {
        print(__FUNCTION__)
    }
    
}

let yjs = YJSomeClass()
yjs.test()
```

如果你想在类型中使用类型方法，可以在func前加static。

```swift
static func test()
```

你还可以让该协议只能由class去实现,只需让协议去继承class

```swift
protocol YJSomeProtocol: class
```

&#160;

#代理

在Swift中有各种各样的代理，这些代理都是通过协议来规范的，想知道更多关于代理模式的实现详见我的博文《[23设计模式之代理模式(Proxy)](http://blog.csdn.net/y550918116j/article/details/48605595)》。
&#160;

#继承

协议也支持继承。

```swift
protocol YJAnotherProtocol: YJSomeProtocol {
    
    // 协议可继承

}
```

继承的好处就是我们可以在一些公共的协议上，添加我们的协议，使之具有延伸性。 
&#160;

#可选

在swift中又可选链，也就是‘？’和'!'。在这里我们考虑到这种情况，有一个协议里面有5个方法，其中两个方法是必须的，其他3个方法，继承的类不必强制去实现。

在OC中就有这种机制@optional，在swift也可以这样使用，但是@optional是OC
的特性。因此，我们想使用这种特性的时候，就需要借助@objc。@objc可以理解为swift和oc的桥梁。

```swift
@objc protocol YJSomeProtocol:class {
    
    // class代表只用类才能实现这个协议
    func test()
    
    // @objc:OC特性，代表可以使用optional特性。optional可选的方法
    optional func testOptional()
    
}
```

但是这样对实现类有一定的影响，因为这个特性是OC的。而OC中所有的类都是继承NSObject，故实现这个协议的类也要继承NSObject。

```swift
class YJSomeClass:NSObject, YJSomeProtocol {

    func test() {
        print(__FUNCTION__)
    }
    
}
```

&#160;

#扩展

协议也支持扩展extension，这也相当于可选。只是在扩展的协议中，需要实现方法。

```swift
extension YJSomeProtocol {
    
    func testExtension() {
        print(__FUNCTION__)
    }

}
```

实现类可以实现这个方法，也可以不实现这个方法。

&#160;

----------

#其他

##参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-2 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Protocols总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j