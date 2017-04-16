# 1 概述

Singleton属于创建型模式中的一种，保证一个类仅有一个实例，并提供一个访问它的全局访问点。

# 2 适用性

1. 当类只能有一个实例而且客户可以从一个众所周知的访问点访问它时。
2. 当这个唯一实例应该是通过子类化可扩展的，并且客户应该无需更改代码就能使用一个扩展的实例时。

# 3 参与者

1. **Singleton**：定义一个Instance操作，允许客户访问它的唯一实例。Instance是一个类操作，可能负责创建它自己的唯一实例。

# 4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112705.png)

# 5 代码实现

```swift
import Cocoa

/// 单例
private class Singleton  {
    
    static let shared = Singleton()
    
    init() {
        print("创建\(#file)")
    }
    
}
```

测试

```swift
print(Singleton.shared)
print(Singleton.shared)
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