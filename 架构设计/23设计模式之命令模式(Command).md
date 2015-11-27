[返回目录](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之目录.md)

----------

#1 概述

Command属于行为型模式中的一种，将一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数化；对请求排队或记录请求日志，以及支持可撤消的操作。

#2 适用性

1. 抽象出待执行的动作以参数化某对象。
2. 在不同的时刻指定、排列和执行请求。
3. 支持取消操作。
4. 支持修改日志，这样当系统崩溃时，这些修改可以被重做一遍。
5. 用构建在原语操作上的高层操作构造一个系统。

#3 参与者

1. **Command**：声明执行操作的接口。
2. **ConcreteCommand**：将一个接收者对象绑定于一个动作。调用接收者相应的操作，以实现Execute。
3. **Client**：创建一个具体命令对象并设定它的接收者。
4. **Invoker**：要求该命令执行这个请求。
5. **Receiver**：知道如何实施与执行一个请求相关的操作。任何类都可能作为一个接收者。

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112716.png)

#5 代码实现

```swift
//
//  YJInterpreter.swift
//  DesignPattern
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/27.
//  Copyright © 2015年 阳君. All rights reserved.
//

import Cocoa

/// 声明一个抽象的解释操作，这个协议为抽象语法树中所有的节点所共享
private protocol ExpressionProtocol {
    
    func interpret(context: Context)
    
}

private class AdvanceExpression: ExpressionProtocol {
    
    func interpret(context: Context){
        print("\(context.content) 这是高级解析器!")
    }
}

private class SimpleExpression: ExpressionProtocol {
    
    func interpret(context: Context) {
        print("\(context.content) 这是普通解析器!")
    }
    
}

// MARK: -

/// Context（上下文）包含解释器之外的一些全局信息
private class Context {
    
    /// 全局信息
    var content:String = ""
    /// 解释器数组
    var list: [ExpressionProtocol] = []
    
    // MARK: 增加
    func add(expression: ExpressionProtocol) {
        self.list.append(expression)
    }
    
}

// MARK: -

/// 解释器模式
class YJInterpreter: YJTestProtocol {

    func test() {
        let ctx = Context()
        ctx.content = "Context"
        ctx.add(AdvanceExpression())
        ctx.add(SimpleExpression())
        for ex in ctx.list {
            ex.interpret(ctx)
        }
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