# 1 概述

Builder属于创建型模式中的一种，将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

# 2 适用性

1. 当创建复杂对象的算法应该独立于该对象的组成部分以及它们的装配方式时。
2. 当构造过程必须允许被构造的对象有不同的表示时。

# 3 参与者

1. **Builder**：为创建一个Product对象的各个部件指定抽象接口。
2. **ConcreteBuilder**：实现Builder的接口以构造和装配该产品的各个部件。定义并明确它所创建的表示,提供一个检索产品的接口。
3. **Director**：构造一个使用Builder接口的对象。
4. **Product**：表示被构造的复杂对象。ConcreteBuilder创建该产品的内部表示并定义它的装配过程。包含定义组成部件的类，包括将这些部件装配成最终产品的接口。

# 4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112703.png)

# 5 代码实现

```swift
import Cocoa

/// Person 表示被构造的复杂对象
private class Person {
    /// 头
    var head:String = ""
    /// 身体
    var body:String = ""
    /// 脚
    var foot:String = ""
}

/// 男人
private class Man: Person {
}

// MARK: -

/// PersonBuilder为创建一个Person对象的各个部件指定抽象协议
private protocol PersonBuilder {
    /// 造头
    func buildHead()    
    /// 造身体
    func buildBody()    
    /// 造脚
    func buildFoot()    
    /// 造人
    func buildPerson() -> Person    
}


/// ManBuilder实现PersonBuilder的协议以构造和装配该产品的各个部件。
private class ManBuilder: PersonBuilder {
    
    // 定义并明确它所创建的表示。提供一个检索产品的协议。
    
    /// 人员对象
    let person:Person = Man()
    
    // MARK: 造头
    func buildHead() {
        person.head = "建造男人的头"
        print(person.head)
    }
    
    // MARK: 造身体
    func buildBody() {
        person.body = "建造男人的身体"
        print(person.body)
    }
    
    // MARK: 造脚
    func buildFoot() {
        person.foot = "建造男人的脚"
        print(person.foot)
    }
    
    // MARK: 造人
    func buildPerson() -> Person {
        return person
    }
    
}

// MARK: -

/// PersonDirector构造一个使用PersonBuilder协议的对象。
private class PersonDirector {
    
    // MARK: 通过装配器构造人员对象
    func constructPerson(pb:PersonBuilder) -> Person {
        pb.buildHead()
        pb.buildBody()
        pb.buildFoot()
        return pb.buildPerson()
    }
    
}
```

测试

```swift
// 构造器
let pd = PersonDirector()
// 装配器
let mb = ManBuilder()
// 创建对象
let person = pd.constructPerson(mb)
print(person)
```

&#160;

----------

# 其他

## 源代码

[Framework](https://github.com/937447974/Framework)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-27 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog