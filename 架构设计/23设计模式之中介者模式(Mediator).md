#1 概述

Mediator属于行为型模式中的一种，用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

#2 适用性

1. 一组对象以定义良好但是复杂的方式进行通信。产生的相互依赖关系结构混乱且难以理解。
2. 一个对象引用其他很多对象并且直接与这些对象通信,导致难以复用该对象。
3. 想定制一个分布在多个类中的行为，而又不想生成太多的子类。

#3 参与者

1. **Mediator**：中介者定义一个接口用于与各同事（Colleague）对象通信。
2. **ConcreteMediator**：具体中介者通过协调各同事对象实现协作行为。了解并维护它的各个同事。
3. **Colleagueclass**：每一个同事类都知道它的中介者对象。每一个同事对象在需与其他的同事通信的时候，与它的中介者通信

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112718.png)

#5 代码实现

```swift
import Cocoa

/// MediatorProtocol中介者定义一个协议用于与各同事（Colleague）对象通信。
private protocol MediatorProtocol {    
    // MARK: 通知
    func notice(content:String)    
}

/// ConcreteMediator具体中介者通过协调各同事对象实现协作行为。
private class ConcreteMediator: MediatorProtocol {
    
    /// 员工A
    private let ca: ColleagueProtocol = ColleagueA()
    /// 员工B
    private let cb: ColleagueProtocol = ColleagueB()
    
    func notice(content: String) {
        switch(content) {
        case "boss":// 老板来了, 通知员工A
            print("老板来了...")
            ca.action()
        case "client":// 客户来了, 通知前台B
            print("客户来了...")
            cb.action()
        default:
            print("错误通知...")
        }
    }
    
}

// MARK: - 

/// ColleagueProtocol协议
private protocol ColleagueProtocol {    
    // MARK: 通知
    func action()    
}

/// 同事A
private class ColleagueA: ColleagueProtocol {
    
    func action() {
        print("普通员工努力工作")
    }
    
}

/// 同事B
private class ColleagueB: ColleagueProtocol {
    
    func action() {
        print("前台注意了!")
    }
    
}
```

测试

```swift
let med = ConcreteMediator()
// 老板来了
med.notice("boss")
// 客户来了
med.notice("client")
// 通知错误
med.notice("other")
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