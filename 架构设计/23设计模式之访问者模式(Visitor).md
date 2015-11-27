[返回目录](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之目录.md)

----------

#1 概述

Visitor属于行为型模式中的一种，表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

#2 适用性

1. 一个对象结构包含很多类对象，它们有不同的接口，而你想对这些对象实施一些依赖于其具体类的操作。
2. 需要对一个对象结构中的对象进行很多不同的并且不相关的操作，而你想避免让这些操作“污染”这些对象的类。Visitor使得你可以将相关的操作集中起来定义在一个类中。当该对象结构被很多应用共享时，用Visitor模式让每个应用仅包含需要用到的操作。
3. 定义对象结构的类很少改变，但经常需要在此结构上定义新的操作。改变对象结构类需要重定义对所有访问者的接口，这可能需要很大的代价。如果对象结构类经常改变，那么可能还是在这些类中定义这些操作较好。
        
#3 参与者

1. **Visitor**：为该对象结构中ConcreteElement的每一个类声明一个Visit操作。该操作的名字和特征标识了发送Visit请求给该访问者的那个类。这使得访问者可以确定正被访问元素的具体的类。这样访问者就可以通过该元素的特定接口直接访问它。
2. **ConcreteVisitor**：实现每个由Visitor声明的操作。每个操作实现本算法的一部分，而该算法片断乃是对应于结构中对象的类。ConcreteVisitor为该算法提供了上下文并存储它的局部状态。这一状态常常在遍历该结构的过程中累积结果。
3. **Element**：定义一个Accept操作，它以一个访问者为参数。
4. **ConcreteElement**：实现Accept操作，该操作以一个访问者为参数。
5. **ObjectStructure**：能枚举它的元素。:可以提供一个高层的接口以允许该访问者访问它的元素。可以是一个复合或是一个集合，如一个列表或一个无序集合。

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112723.png)

#5 代码实现

```swift
//
//  YJVisitor.swift
//  DesignPattern
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/26.
//  Copyright © 2015年 阳君. All rights reserved.
//

import Cocoa

/// VisitableProtocol定义一个Accept操作，它以一个访问者为参数。
private protocol VisitableProtocol {
    
    func accept(visitor: VisitorProtocol)
    
}

/// FloatElement实现VisitableProtocol操作，该操作以一个访问者为参数
private class FloatElement: VisitableProtocol {
    
    private var fe:Float
    
    init(fe:Float) {
        self.fe = fe
    }
    
    func accept(visitor: VisitorProtocol) {
        visitor.visitFloat(self)
    }
    
}

/// StringElement实现VisitableProtocol操作，该操作以一个访问者为参数
private class StringElement: VisitableProtocol {
    
    private var se:String
    
    init(se:String) {
        self.se = se
    }
    
    func accept(visitor: VisitorProtocol) {
        visitor.visitString(self)
    }
    
}

// MARK: - 

/// VisitorProtocol为该对象结构中ConcreteElement的每一个类声明一个Visit操作
private protocol VisitorProtocol {
    
    func visitString(stringE: StringElement)
    
    func visitFloat(floatE: FloatElement)
    
    func visitCollection(collection: Array<VisitableProtocol>)
    
}

/// ConcreteVisitor实现每个由Visitor声明的操作
private class ConcreteVisitor: VisitorProtocol {
    
    func visitFloat(floatE: FloatElement) {
        print(floatE.fe)
    }
    
    func visitString(stringE: StringElement) {
        print(stringE.se)
    }
    
    func visitCollection(collection: Array<VisitableProtocol>) {
        for visitable in collection {
            visitable.accept(self)
        }
    }
    
}

// MARK: - 

/// 访问者模式
class YJVisitor: YJTestProtocol {

    func test() {
        let visitor = ConcreteVisitor()
        let se = StringElement(se: "abc")
        se.accept(visitor)
        let fe = FloatElement(fe: 1.5)
        fe.accept(visitor)
        print("====")
        let list: Array<VisitableProtocol> = [se, fe]
        visitor.visitCollection(list)
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