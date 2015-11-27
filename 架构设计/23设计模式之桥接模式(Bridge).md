[返回目录](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之目录.md)

----------

#1 概述

Bridge属于结构型模式中的一种，将抽象部分与它的实现部分分离，使它们都可以独立地变化。

#2 适用性

1. 你不希望在抽象和它的实现部分之间有一个固定的绑定关系。例如这种情况可能是因为，在程序运行时刻实现部分应可以被选择或者切换。
2. 类的抽象以及它的实现都应该可以通过生成子类的方法加以扩充。这时Bridge模式使你可以对不同的抽象接口和实现部分进行组合，并分别对它们进行扩充。
3. 对一个抽象的实现部分的修改应对客户不产生影响，即客户的代码不必重新编译。
4. 正如在意图一节的第一个类图中所示的那样，有许多类要生成。这样一种类层次结构说明你必须将一个对象分解成两个部分。
5. 你想在多个对象间共享实现（可能使用引用计数），但同时要求客户并不知道这一点。

#3 参与者

1. **Abstraction**：定义抽象类的接口。维护一个指向Implementor类型对象的指针。
2. **RefinedAbstraction**：扩充由Abstraction定义的接口。
3. **Implementor**：定义实现类的接口，该接口不一定要与Abstraction的接口完全一致。事实上这两个接口可以完全不同。一般来讲，Implementor接口仅提供基本操作，而Abstraction则定义了基于这些基本操作的较高层次的操作。
4. **ConcreteImplementor**：实现Implementor接口并定义它的具体实现。

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112707.png)

#5 代码实现

```swift
//
//  YJBridge.swift
//  DesignPattern
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/26.
//  Copyright © 2015年 阳君. All rights reserved.
//

import Cocoa

/// Clothing定义实现类的协议，该协议不一定要与PersonBr的协议完全一致
private protocol Clothing {
    
    func personDressCloth(person:Person)
    
}

/// Trouser实现Clothing协议并定义它的具体实现。
private class Trouser: Clothing {
    
    func personDressCloth(person: Person) {
        print("\(person.type)穿裤子");
    }
    
}

/// Clothes实现Clothing协议并定义它的具体实现
private class Clothes: Clothing {
    
    func personDressCloth(person: Person) {
        print("\(person.type)穿衣服");
    }
    
}

// MARK: -

/// PersonBr定义抽象类的协议。
private class Person {
    
    /// 服装
    var clothing:Clothing?
    /// 标示
    var type:String = ""
    
    /// 桥接的动作
    func dress() {
        
    }
    
}

/// Man扩充由Person定义的协议
private class Man: Person {
    
    override init() {
        super.init()
        super.type = "男人"
    }
    
    override func dress() {
        super.clothing?.personDressCloth(self)
    }
    
}

/// Lady扩充由Person定义的协议
private class Lady: Person {
    
     override init() {
        super.init()
        self.type = "女人"
    }
    
    override func dress() {
        super.clothing?.personDressCloth(self)
    }
    
}

// MARK: -

/// 桥接模式
class YJBridge: YJTestProtocol {

    func test() {
        let clothes: Clothing = Clothes()
        let trouser: Clothing = Trouser()
        
        let man: Person = Man()
        let lady: Person = Lady()
        
        man.clothing = clothes
        man.dress()
        man.clothing = trouser
        man.dress()
        // 或
        clothes.personDressCloth(lady)
        trouser.personDressCloth(lady)
    }
    
}
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