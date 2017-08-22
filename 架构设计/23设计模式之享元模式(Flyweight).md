# 1 概述

Flyweight属于结构型模式中的一种，运用共享技术有效地支持大量细粒度的对象。

# 2 适用性

当都具备下列情况时，使用Flyweight模式：

1. 一个应用程序使用了大量的对象。
2. 完全由于使用大量的对象，造成很大的存储开销。
3. 对象的大多数状态都可变为外部状态。
4. 如果删除对象的外部状态，那么可以用相对较少的共享对象取代很多组对象。
5. 应用程序不依赖于对象标识。由于Flyweight对象可以被共享，对于概念上明显有别的对象，标识测试将返回真值。

# 3 参与者

1. **Flyweight**：描述一个接口，通过这个接口flyweight可以接受并作用于外部状态。
2. **ConcreteFlyweight**：实现Flyweight接口，并为内部状态（如果有的话）增加存储空间。ConcreteFlyweight对象必须是可共享的。它所存储的状态必须是内部的；即，它必须独立于ConcreteFlyweight对象的场景。
3. **UnsharedConcreteFlyweight**：并非所有的Flyweight子类都需要被共享。Flyweight接口使共享成为可能，但它并不强制共享。在Flyweight对象结构的某些层次，UnsharedConcreteFlyweight对象通常将ConcreteFlyweight对象作为子节点。
4. **FlyweightFactory**：创建并管理flyweight对象。确保合理地共享flyweight。当用户请求一个flyweight时，FlyweightFactory对象提供一个已创建的实例或者创建一个（如果不存在的话）。

# 4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112711.png)

# 5 代码实现

```swift
import Cocoa

/// FlyweightProtocol:描述一个协议，通过这个协议flyweight可以接受并作用于外部状态。
private protocol FlyweightProtocol {
    func action(arg: Int)    
}

/// ConcreteFlyweight:实现Flyweight协议，并为内部状态（如果有的话）增加存储空间。
private class Flyweight: FlyweightProtocol {
    
    // ConcreteFlyweight对象必须是可共享的。它所存储的状态必须是内部的；
    // 即，它必须独立于ConcreteFlyweight对象的场景。
    
    func action(arg: Int) {
        print("参数值: \(arg)")
    }
    
}

// MARK: -

/// FlyweightFactory创建并管理flyweight对象
private class FlyweightFactory  {
    
    private static var flyweights = Dictionary<String, FlyweightProtocol>()
    
    init(arg:String){
        FlyweightFactory.flyweights[arg] = Flyweight()
    }
    
    // MARK: 获取Flyweight类
    class func getFlyweight(key:String) -> FlyweightProtocol {
        if (self.flyweights[key] == nil) {
            self.flyweights[key] = Flyweight()
        }
        return self.flyweights[key]!
    }
    
    // MARK: 获取当前存储的对象个数
    class func getSize() ->Int {
        return self.flyweights.count;
    }
    
}
```

测试

```swift
// 享元模式
let fly1 = FlyweightFactory.getFlyweight("a")
fly1.action(1)
let fly2 = FlyweightFactory.getFlyweight("a")
fly2.action(2);
let fly3 = FlyweightFactory.getFlyweight("b")
fly3.action(2);
print("对象个数：\(FlyweightFactory.getSize())");
```

&#160;

----------

# 其他

## 源代码

[Framework](https://github.com/937447974/Framework)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-27 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog