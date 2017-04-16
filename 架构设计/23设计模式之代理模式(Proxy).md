# 1 概述

Proxy属于结构型模式中的一种，为其他对象提供一种代理以控制对这个对象的访问。

# 2 适用性

1. 远程代理（RemoteProxy）为一个对象在不同的地址空间提供局部代表。
2. 虚代理（VirtualProxy）根据需要创建开销很大的对象。
3. 保护代理（ProtectionProxy）控制对原始对象的访问。
4. 智能指引（SmartReference）取代了简单的指针，它在访问对象时执行一些附加操作。

# 3 参与者

1. **Proxy**：保存一个引用使得代理可以访问实体。若RealSubject和Subject的接口相同，Proxy会引用Subject。提供一个与Subject的接口相同的接口，这样代理就可以用来替代实体。控制对实体的存取，并可能负责创建和删除它。其他功能依赖于代理的类型：
2. **RemoteProxy**：负责对请求及其参数进行编码，并向不同地址空间中的实体发送已编码的请求。
3. **VirtualProxy**：可以缓存实体的附加信息，以便延迟对它的访问。
4. **ProtectionProxy**：检查调用者是否具有实现一个请求所必需的访问权限。
5. **Subject**：定义RealSubject和Proxy的共用接口，这样就在任何使用RealSubject的地方都可以使用Proxy。
6. **RealSubject**：定义Proxy所代表的实体。

# 4 类图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112712.png)

# 5 代码实现

```swift
import Cocoa

/// ObjectProtocol定义ViewController和ProxyObject的共用协议
private protocol ObjectProtocol {
    func action()
}

/// ProxyObject保存一个引用使得代理可以访问实体。
private class ProxyObject {
    
    /// 被代理类
    var objProtocol: ObjectProtocol?
    
    // MARK: 代理方法实现
    func action() {
        print("send action begin");
        self.objProtocol?.action();
        print("send action end");
    }
    
}

/// RealSubject定义Proxy所代表的实体。
private class ViewController: ObjectProtocol {
    
    func action() {
        print("get action");
    }
    
}
```

测试

```swift
let vc = ViewController() // 当前VC
let obj = ProxyObject()
obj.objProtocol = vc // 设置代理类
obj.action() // 发送消息
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