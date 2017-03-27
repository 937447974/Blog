#1 概述

Interpreter属于行为型模式中的一种，给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。

#2 适用性

当有一个语言需要解释执行,并且你可将该语言中的句子表示为一个抽象语法树时，可使用解释器模式。而当存在以下情况时该模式效果最好：

1. 该文法简单对于复杂的文法,文法的类层次变得庞大而无法管理。
2. 效率不是一个关键问题最高效的解释器通常不是通过直接解释语法分析树实现的,而是首先将它们转换成另一种形式。

#3 参与者

1. **AbstractExpression(抽象表达式)**：声明一个抽象的解释操作，这个接口为抽象语法树中所有的节点所共享。
2. **TerminalExpression(终结符表达式)**：实现与文法中的终结符相关联的解释操作。一个句子中的每个终结符需要该类的一个实例。
3. **NonterminalExpression(非终结符表达式)**：为文法中的非终结符实现解释(Interpret)操作。
4. **Context（上下文）**：包含解释器之外的一些全局信息。
5. **Client（客户）**：构建(或被给定)表示该文法定义的语言中一个特定的句子的抽象语法树。该抽象语法树由NonterminalExpression和TerminalExpression的实例装配而成。调用解释操作。

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112713.png)

#5 代码实现

```swift
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
```

测试

```swift
let ctx = Context()
ctx.content = "Context"
ctx.add(AdvanceExpression())
ctx.add(SimpleExpression())
for ex in ctx.list {
	ex.interpret(ctx)
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