#1 概述

Decorator属于结构型模式中的一种，动态地给一个对象添加一些额外的职责。就增加功能来说，Decorator模式相比生成子类更为灵活。

#2 适用性

1. 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责。
2. 处理那些可以撤消的职责。
3. 当不能采用生成子类的方法进行扩充时。

#3 参与者

1. **Component**：定义一个对象接口，可以给这些对象动态地添加职责。
2. **ConcreteComponent**：定义一个对象，可以给这个对象添加一些职责。
3. **Decorator**：维持一个指向Component对象的指针，并定义一个与Component接口一致的接口。
4. **ConcreteDecorator**：向组件添加职责。

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112709.png)

#5 代码实现

```swift
import Cocoa

/// PersonProtocol定义一个对象协议，可以给这些对象动态地添加职责。
private protocol PersonProtocol {    
    func eat()    
}

/// Man定义一个对象，可以给这个对象添加一些职责
private class Man: PersonProtocol {
    
    func eat() {
        print("男人在吃")
    }
    
}

// MARK: -

/// Decorator维持一个指向PersonProtocol对象的指针，并实现PersonProtocol协议
private class Decorator: PersonProtocol {
    
    /// 协议对象，等待实现
    var person: PersonProtocol?
    
    func eat() {
        person?.eat()
    }
    
}

/// ManDecoratorA向组件添加职责
private class ManDecoratorA: Decorator {
    
    override func eat() {
        super.eat()
        print("ManDecoratorA类");
    }
}

/// ManDecoratorB向组件添加职责
private class ManDecoratorB: Decorator {
    
    override func eat() {
        super.eat();
        print("ManDecoratorB类");
    }
    
}
```

测试

```swift
let man = Man()
let md1 = ManDecoratorA()
let md2 = ManDecoratorB()
md1.person = man
md2.person = md1
md2.eat()
```

&#160;

----------

#其他

##源代码

[Framework](https://github.com/937447974/Framework)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-27 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog