[返回目录](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之目录.md)

----------

#1 概述

TemplateMethod属于行为型模式中的一种，定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。   
TemplateMethod使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

#2 适用性

1. 一次性实现一个算法的不变的部分，并将可变的行为留给子类来实现。
2. 各子类中公共的行为应被提取出来并集中到一个公共父类中以避免代码重复。首先识别现有代码中的不同之处，并且将不同之处分离为新的操作。最后，用一个调用这些新的操作的模板方法来替换这些不同的代码。
3. 控制子类扩展。

#3 参与者

1. **AbstractClass**：定义抽象的原语操作（primitiveoperation），具体的子类将重定义它们以实现一个算法的各步骤。实现一个模板方法,定义一个算法的骨架。该模板方法不仅调用原语操作，也调用定义在AbstractClass或其他对象中的操作。
2. **ConcreteClass**：实现原语操作以完成算法中与特定子类相关的步骤。

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112714.png)

#5 代码实现

```swift
//
//  YJTemplateMethod.swift
//  DesignPattern
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/27.
//  Copyright © 2015年 阳君. All rights reserved.
//

import Cocoa

/// Template定义抽象的原语操作，具体的子类将重定义它们以实现一个算法的各步骤。
private class Template {
    
    // MARK: 子类实现
    func println() {
        
    }
    
    // MARK: 原始方法
    func update() {
        print("开始打印")
        for (var i = 0; i < 3; i++) {
            self.println()
        }
    }
    
}

/// TemplateConcrete实现原语操作以完成算法中与特定子类相关的步骤
private class TemplateConcrete: Template {
    
    override func println() {
        print("这是子类的实现")
    }
    
}

// MARK: - 

/// 模板方法
class YJTemplateMethod: YJTestProtocol {

    func test() {
        let temp: Template = TemplateConcrete()
        temp.update()
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