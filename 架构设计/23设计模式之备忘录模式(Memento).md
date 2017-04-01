#1 概述

Memento 属于行为型模式中的一种，在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保存的状态。

#2 适用性

1. 必须保存一个对象在某一个时刻的(部分)状态，这样以后需要时它才能恢复到先前的状态。
2. 如果一个用接口来让其它对象直接得到这些状态，将会暴露对象的实现细节并破坏对象的封装性。

#3 参与者

1. **Memento**：备忘录存储原发器对象的内部状态。
2. **Originator**：原发器创建一个备忘录，用以记录当前时刻它的内部状态。使用备忘录恢复内部状态。
3. **Caretaker**：负责保存好备忘录。不能对备忘录的内容进行操作或检查。

#4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112719.png)

#5 代码实现

```swift
import Cocoa

/// Memento备忘录存储原发器对象的内部状态
private class Memento {    
    var state: String?
}

/// Caretaker负责保存好备忘录,不能对备忘录的内容进行操作或检查
private class Caretaker {    
    var memento: Memento?    
}

/// Originator原发器创建一个备忘录,用以记录当前时刻它的内部状态。使用备忘录恢复内部状态
private class Originator {
    
    fileprivate var state: String?
    
    // MARK: 数据封装
    func createMemento() -> Memento {
        let memento = Memento()
        memento.state = self.state
        return memento
    }
    
    // MARK: 将数据重新导入
    func setMemento(_ memento: Memento) {
        self.state = memento.state
    }
    
    // MARK: 显示
    func showState() {
        print("\(self.state)")
    }
}
```

测试

```swift
let org = Originator()
org.state = "开会中"
let ctk = Caretaker()
// 将数据封装在Caretaker
ctk.memento = org.createMemento()
org.state = "睡觉中"
org.showState()// 显示
org.setMemento(ctk.memento!)//将数据重新导入
org.showState()
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