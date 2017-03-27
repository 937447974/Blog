#1 概述

Prototype属于创建型模式中的一种，用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。

#2 适用性

1. 当一个系统应该独立于它的产品创建、构成和表示时。
2. 当要实例化的类是在运行时刻指定时，例如，通过动态装载。
3. 为了避免创建一个与产品类层次平行的工厂类层次时。
4. 当一个类的实例只能有几个不同状态组合中的一种时。

#3 参与者

1. **Prototype**：实现一个克隆自身的操作。
2. **ConcretePrototype**：实现一个克隆自身的原型。
3. **Client**：让一个原型克隆自身从而创建一个新的对象。

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112704.png)

#5 代码实现

```swift
import Cocoa

/// 实现一个克隆自身的操作
private class Prototype {

    var name:String = ""
    
    // MARK: 克隆
    func clone() -> Prototype {
        let pro = Prototype()
        pro.name = name
        return pro
    }
}

/// ConcretePrototype 实现一个克隆自身的原型
private class ConcretePrototype: Prototype {
    
    init(name:String) {
        super.init()
        super.name = name
    }
    
}
```

测试

```swift
let pro = ConcretePrototype(name:"阳君")
//  Client,让一个原型克隆自身从而创建一个新的对象。
let pro2 = pro.clone()
print(pro.name)
print(pro2.name)
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