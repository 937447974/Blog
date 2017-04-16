# 1 概述

Iterator属于行为型模式中的一种，给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。

# 2 适用性

1. 访问一个聚合对象的内容而无需暴露它的内部表示。
2. 支持对聚合对象的多种遍历。
3. 为遍历不同的聚合结构提供一个统一的接口(即,支持多态迭代)。

# 3 参与者

1. **Iterator**：迭代器定义访问和遍历元素的接口。
2. **ConcreteIterator**：具体迭代器实现迭代器接口。对该聚合遍历时跟踪当前位置。
3. **Aggregate**：聚合定义创建相应迭代器对象的接口。
4. **ConcreteAggregate**：具体聚合实现创建相应迭代器的接口，该操作返回ConcreteIterator的一个适当的实例。

# 4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112718.png)

# 5 代码实现

```swift
import Cocoa

/// 聚合定义创建相应迭代器对象的协议
private protocol ListProtocol {    
    func iterator() -> IteratorProtocol    
    func get(index:Int) -> AnyObject    
    func getSize() -> Int    
    func add(obj:AnyObject)    
}

/// 具体聚合实现创建相应迭代器的协议，该操作返回ConcreteIterator的一个适当的实例.
private class List: ListProtocol {
    
    /// 数组
    private var list:[AnyObject] = []
    /// 位置
    private var index:Int = 0
    /// 长度
    private var size:Int = 0
    
    func iterator() -> IteratorProtocol {
        return Iterator(list: self)
    }
    
    func get(index:Int) ->AnyObject {
        return list[index]
    }
    
    func getSize() ->Int {
        return size
    }
    
    func add(obj:AnyObject) {
        list.append(obj)
        index += 1
        size += 1
    }
    
}

// MARK: - 

/// 迭代器定义访问和遍历元素的协议
private protocol IteratorProtocol {    
    func next() ->AnyObject
    func hasNext() ->Bool    
}

/// 具体迭代器实现迭代器协议,对该聚合遍历时跟踪当前位置
private class Iterator: IteratorProtocol {
    
    /// 数组
    private var list:List
    /// 位置
    private var index:Int
    
    init(list:List) {
        self.index = 0
        self.list = list
    }
    
    func next() -> AnyObject {
        let obj:AnyObject = self.list.get(index)
        self.index += 1
        return obj
    }
    
    func hasNext() -> Bool {
        return self.index < self.list.getSize()
    }
    
}

// MARK: - 

/// 迭代器
class YJIterator: YJTestProtocol {

    func test() {
        let list = List()
        list.add("a")
        list.add("b")
        list.add("c")
        //第一种迭代方式
        let it = list.iterator();
        while (it.hasNext()) {
            print(it.next())
        }
        print("===========")
        //第二种迭代方式
        for (var i = 0; i < list.getSize(); i++) {
            print(list.get(i));
        }
    }
    
}
```

测试

```swift
let list = List()
list.add("a" as AnyObject)
list.add("b" as AnyObject)
list.add("c" as AnyObject)
//第一种迭代方式
let it = list.iterator();
while (it.hasNext()) {
	print(it.next())
}
print("===========")
//第二种迭代方式
for i in 0 ..< list.getSize() {
	print(list.get(i));
}
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