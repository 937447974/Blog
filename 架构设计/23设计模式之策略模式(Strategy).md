[返回目录](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之目录.md)

----------

#1 概述

Strategy属于行为型模式中的一种，定义一系列的算法,把它们一个个封装起来,并且使它们可相互替换。本模式使得算法可独立于使用它的客户而变化。

#2 适用性

1. 许多相关的类仅仅是行为有异。“策略”提供了一种用多个行为中的一个行为来配置一个类的方法。
2. 需要使用一个算法的不同变体。
3. 算法使用客户不应该知道的数据。可使用策略模式以避免暴露复杂的、与算法相关的数据结构。
4. 一个类定义了多种行为,并且这些行为在这个类的操作中以多个条件语句的形式出现。将相关的条件分支移入它们各自的Strategy类中以代替这些条件语句。

#3 参与者

1. **Strategy**：定义所有支持的算法的公共接口。Context使用这个接口来调用某ConcreteStrategy定义的算法。
2. **ConcreteStrategy**：以Strategy接口实现某具体算法。
3. **Context**：用一个ConcreteStrategy对象来配置。维护一个对Strategy对象的引用。可定义一个接口来让Stategy访问它的数据。

#4 类图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112722.png)

#5 代码实现

```swift
//
//  YJStrategy.swift
//  DesignPattern
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/26.
//  Copyright © 2015年 阳君. All rights reserved.
//

import Cocoa

/// StrategyProtocol定义所有支持的算法的公共协议
private protocol StrategyProtocol {
    
    func method()
    
}

/// 以StrategyProtocol协议实现某具体算法
private class StrategyA: StrategyProtocol {
    
    func method() {
        print("StrategyA实现");
    }
    
}

/// 以StrategyProtocol协议实现某具体算法
private class StrategyB: StrategyProtocol {
    
    func method() {
        print("StrategyB实现");
    }
    
}

// MARK: -

/// 维护一个对Strategy对象的引用
private class ContextSt {
    
    var strategy: StrategyProtocol?
    
    func doMethod() {
        self.strategy?.method()
    }
    
}

// MARK: -

/// 策略模式
class YJStrategy: YJTestProtocol {

    func test() {
        let ctxSt = ContextSt()
        ctxSt.strategy = StrategyA()
        ctxSt.doMethod()
        ctxSt.strategy = StrategyB()
        ctxSt.doMethod()
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