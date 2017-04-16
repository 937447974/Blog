# 1 概述

当一个对象的内在状态改变时允许改变其行为，这个对象看起来像是改变了其类。

# 2 适用性

1. 一个对象的行为取决于它的状态,并且它必须在运行时刻根据状态改变它的行为。
2. 一个操作中含有庞大的多分支的条件语句，且这些分支依赖于该对象的状态。这个状态通常用一个或多个枚举常量表示。通常,有多个操作包含这一相同的条件结构。
3. State模式将每一个条件分支放入一个独立的类中。这使得你可以根据对象自身的情况将对象的状态作为一个对象，这一对象可以不依赖于其他对象而独立变化。

# 3 参与者

1. **Context**：定义客户感兴趣的接口。维护一个ConcreteState子类的实例，这个实例定义当前状态。
2. **State**：定义一个接口以封装与Context的一个特定状态相关的行为。
3. **ConcreteStatesubclasses**：每一子类实现一个与Context的一个状态相关的行为

# 4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112721.png)

# 5 代码实现

```swift
import Cocoa

/// WeatherProtocol定义一个协议以封装与Context的一个特定状态相关的行为
private protocol WeatherProtocol {    
    func getWeather() ->String    
}

private class Rain: WeatherProtocol {
    
    func getWeather() -> String {
        return "下雨"
    }
    
}

private class Sunshine: WeatherProtocol {
    
    func getWeather() -> String {
        return "阳光"
    }
    
}

// MARK: -

/// Context定义客户感兴趣的协议
private class Context {
    
    var weather: WeatherProtocol?
    
    func weatherMessage() ->String?{
        return self.weather?.getWeather()
    }
    
}
```

测试

```swift
let ctx = Context()
print(ctx.weatherMessage())
ctx.weather = Sunshine()
print(ctx.weatherMessage())
// 改变状态
ctx.weather = Rain()
print(ctx.weatherMessage())
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