# 1 概述

**抽象工厂(Abstract Factory)**属于创建型模式中的一种，提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。

# 2 适用性

1. 一个系统要独立于它的产品的创建、组合和表示时。
2. 一个系统要由多个产品系列中的一个来配置时。
3. 当你要强调一系列相关的产品对象的设计以便进行联合使用时。
4. 当你提供一个产品类库，而只想显示它们的接口而不是实现时。

# 3 参与者

1. **AbstractFactory**：声明一个创建抽象产品对象的操作接口。
2. **ConcreteFactory**：实现创建具体产品对象的操作。
3. **AbstractProduct**：为一类产品对象声明一个接口。
4. **ConcreteProduct**：定义一个将被相应的具体工厂创建的产品对象，实现AbstractProduct接口。
5. **Client**：仅使用由AbstractFactory和AbstractProduct类声明的接口

# 4 类图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112702.png)

# 5 代码实现

```swift
import Cocoa

/// CatProtocol为一类产品对象声明一个协议。
private protocol CatProtocol {
    func eat()
}

/// WhiteCat定义一个将被相应的具体工厂创建的产品对象。实现CatProtocol协议
private class WhiteCat: CatProtocol {
    
    func eat() {
        print("The white cat is eating!")
    }
    
}

/// BlackCat定义一个将被相应的具体工厂创建的产品对象。实现CatProtocol协议
private class BlackCat: CatProtocol {
    
    func eat() {
        print("The black cat is eating!")
    }
    
}

// MARK: -

/// DogProtocol为一类产品对象声明一个协议。
private protocol DogProtocol {
    func eat()
}

/// WhiteDog定义一个将被相应的具体工厂创建的产品对象。实现DogProtocol协议
private class WhiteDog: DogProtocol {
    
    func eat() {
        print("The white dog is eating!")
    }
    
}

/// BlackDog定义一个将被相应的具体工厂创建的产品对象。实现DogProtocol协议
private class BlackDog: DogProtocol {
    
    func eat() {
        print("The black dog is eating!")
    }
    
}

// MARK: -

/// AnimalFactoryProtocol声明一个创建抽象产品对象的操作协议。
private protocol AnimalFactoryProtocol {
    
    /// 创建ICat
    func createCat() -> CatProtocol
    
    /// 创建IDog
    func createDog() -> DogProtocol
    
}

/// WhiteAnimalFactory 实现创建具体产品对象的操作。
private class WhiteAnimalFactory: AnimalFactoryProtocol {
    
    func createCat() -> CatProtocol {
        return WhiteCat()
    }
    
    func createDog() -> DogProtocol {
        return WhiteDog()
    }
    
}

/// BlackAnimalFactory 实现创建具体产品对象的操作。
private class BlackAnimalFactory: AnimalFactoryProtocol {
    
    func createCat() -> CatProtocol {
        return BlackCat()
    }
    
    func createDog() -> DogProtocol {
        return BlackDog()
    }
    
}
```

测试

```swift
let blackAnimalFactory = BlackAnimalFactory()
let whiteAnimalFactory = WhiteAnimalFactory()
    
var cat = blackAnimalFactory.createCat()
cat.eat()
cat = whiteAnimalFactory.createCat()
cat.eat()
    
var dog = blackAnimalFactory.createDog()
dog.eat()
dog = whiteAnimalFactory.createDog()
dog.eat()
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