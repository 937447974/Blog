[返回目录](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之目录.md)

----------

#1 概述

FactoryMethod属于创建型模式中的一种，定义一个用于创建对象的接口，让子类决定实例化哪一个类。FactoryMethod使一个类的实例化延迟到其子类。

#2 适用性

1. 当一个类不知道它所必须创建的对象的类的时候。
2. 当一个类希望由它的子类来指定它所创建的对象的时候。
3. 当类将创建对象的职责委托给多个帮助子类中的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时候。

#3 参与者

1. **Product**：定义工厂方法所创建的对象的接口。
2. **ConcreteProduct**：实现Product接口。
3. **Creator**：声明工厂方法，该方法返回一个Product类型的对象。Creator也可以定义一个工厂方法的缺省实现，它返回一个缺省的ConcreteProduct对象。可以调用工厂方法以创建一个Product对象。
4. **ConcreteCreator**：重定义工厂方法以返回一个ConcreteProduct实例。

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112701.png)

#5 代码实现

```swift
//
//  YJFactoryMethod.swift
//  DesignPattern
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/26.
//  Copyright © 2015年 阳君. All rights reserved.
//

import Cocoa

/// WorkProtocol定义工厂方法所创建的对象的协议。
private protocol WorkProtocol {
    
    func doWork()
}

/// StudentWork实现WorkProtocol协议。
private class StudentWork : WorkProtocol {
    
    func doWork() {
        print("阳君同学做作业!")
    }
    
}

/// TeacherWork实现WorkProtocol协议。
private class TeacherWork : WorkProtocol {
    
    func doWork() {
        print("王老师审批作业!")
    }
    
}

// MARK: -

/// WorkFactoryProtocol声明工厂方法，该方法返回一个支持WorkProtocol协议的对象。
private protocol WorkFactoryProtocol {
    
    // WorkFactoryProtocol也可以定义一个工厂方法的缺省实现，它返回一个缺省的WorkProtocol对象。
    // 可以调用工厂方法以创建一个Work对象。
    
    func getWork() -> WorkProtocol
    
}

/// 重定义工厂方法以返回一个StudentWork实例。
private class StudentWorkFactory : WorkFactoryProtocol {
    
    func getWork() -> WorkProtocol {
        return StudentWork()
    }
    
}

/// 重定义工厂方法以返回一个TeacherWork实例。
private class TeacherWorkFactory : WorkFactoryProtocol {
    
    func getWork() -> WorkProtocol {
        return TeacherWork()
    }
    
}

// MARK: -

/// 工厂方法测试
class YJFactoryMethod: YJTestProtocol {
    
    func test() {
        let studentWorkFactory = StudentWorkFactory()
        studentWorkFactory.getWork().doWork()
        let teacherWorkFactory = TeacherWorkFactory()
        teacherWorkFactory.getWork().doWork()
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