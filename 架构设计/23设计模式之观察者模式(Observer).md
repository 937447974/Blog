[返回目录](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之目录.md)

----------

#1 概述

Observer属于行为型模式中的一种，定义对象间的一种一对多的依赖关系,当一个对象的状态发生改变时,所有依赖于它的对象都得到通知并被自动更新。

#2 适用性

1. 当一个抽象模型有两个方面,其中一个方面依赖于另一方面。将这二者封装在独立的对象中以使它们可以各自独立地改变和复用。
2. 当对一个对象的改变需要同时改变其它对象,而不知道具体有多少对象有待改变。
3. 当一个对象必须通知其它对象，而它又不能假定其它对象是谁。

#3 参与者

1. **Subject（目标）**：目标知道它的观察者。可以有任意多个观察者观察同一个目标。提供注册和删除观察者对象的接口。
2. **Observer（观察者）**：为那些在目标发生改变时需获得通知的对象定义一个更新接口。
3. **ConcreteSubject（具体目标）**：将有关状态存入各ConcreteObserver对象。当它的状态发生改变时,向它的各个观察者发出通知。
4. **ConcreteObserver（具体观察者）**：维护一个指向ConcreteSubject对象的引用。存储有关状态，这些状态应与目标的状态保持一致。实现Observer的更新接口以使自身状态与目标的状态保持一致。

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112720.png)

#5 代码实现

```swift
//
//  YJObserver.swift
//  DesignPattern
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/27.
//  Copyright © 2015年 阳君. All rights reserved.
//

import Cocoa

/// PolicemanProtocol（观察者）为那些在目标发生改变时需获得通知的对象定义一个更新协议。
private protocol PolicemanProtocol {
    
    func action(citizen:Citizen)
    
}

/// HuangPuPoliceman具体观察者
private class HuangPuPoliceman: PolicemanProtocol {
    
    func action(citizen: Citizen) {
        switch (citizen.help) {
        case "normal":
            print("一切正常, 不用出动!")
        case "unnormal":
            print("有犯罪行为, 黄埔警察出动!")
        default:
            print("default...")
        }
    }
}

/// HuangPuPoliceman具体观察者
private class TianHePoliceman: PolicemanProtocol {
    
    func action(citizen: Citizen) {
        switch (citizen.help) {
        case "normal":
            print("一切正常, 不用出动!")
        case "unnormal":
            print("有犯罪行为, 天河警察出动!")
        default:
            print("default...")
        }
    }
    
}

// MARK: - 

/// Citizen（目标）目标知道它的观察者。可以有任意多个观察者观察同一个目标。提供注册和删除观察者对象的协议
private class Citizen {
    
    /// 警察局列表
    var pols: [PolicemanProtocol] = []
    /// 通知信息
    var help: String = "normal"
    
    // MARK: 子类实现
    func sendMessage(help: String) {
        
    }
    
    // MARK: 注册
    func register(pol: PolicemanProtocol) {
        self.pols.append(pol);
    }
    
}

/// 将有关状态存入HuangPuCitizen对象。当它的状态发生改变时,向它的各个观察者发出通知
private class HuangPuCitizen: Citizen {
    
    init(pol: PolicemanProtocol) {
        super.init()
        self.register(pol)
    }
    
    override func sendMessage(help: String) {
        self.help = help
        for pol in self.pols {
            //通知警察行动
            pol.action(self)
        }
    }
    
}

/// 将有关状态存入TianHeCitizen对象。当它的状态发生改变时,向它的各个观察者发出通知
private class TianHeCitizen: Citizen {
    
    init(pol: PolicemanProtocol) {
        super.init()
        self.register(pol)
    }
    
    override func sendMessage(help: String) {
        self.help = help
        for pol in self.pols {
            //通知警察行动
            pol.action(self)
        }
    }
    
}

// MARK: -

/// 观察者模式
class YJObserver: YJTestProtocol {

    func test() {
        let hpPol = HuangPuPoliceman()
        var citizen:Citizen = HuangPuCitizen(pol: hpPol)
        citizen.sendMessage("unnormal")
        let thPol = TianHePoliceman()
        citizen = TianHeCitizen(pol: thPol)
        citizen.sendMessage("normal")
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