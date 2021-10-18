# 1 概述

COR属于行为型模式中的一种，使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止。这一模式的想法是，给多个对象处理一个请求的机会，从而解耦发送者和接受者。

# 2 适用性

1. 有多个的对象可以处理一个请求，哪个对象处理该请求运行时刻自动确定。
2. 你想在不明确指定接收者的情况下，向多个对象中的一个提交一个请求。
3. 可处理一个请求的对象集合应被动态指定。

# 3 参与者

1. **Handler**：定义一个处理请求的接口。（可选）实现后继链。
2. **ConcreteHandler**：处理它所负责的请求。可访问它的后继者。如果可处理该请求，就处理之；否则将该请求转发给它的后继者。
3. **Client**：向链上的具体处理者(ConcreteHandler)对象提交请求。

# 4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112715.png)

# 5 代码实现

```swift
import Cocoa

/// 定义一个处理请求的协议
private protocol RequestHandleProtocol {    
    func handleRequest(request: RequestProtocol)    
}

///  处理它所负责的请求。可访问它的后继者。
private class HRRequestHandle: RequestHandleProtocol {
    
    func handleRequest(request: RequestProtocol) {
        if request is DimissionRequest {
            print("要离职, 人事审批!")
        }
        print("请求完毕")
    }
    
}

///  处理它所负责的请求。可访问它的后继者。
private class PMRequestHandle: RequestHandleProtocol {
    
    var rh: RequestHandleProtocol
    
    init(requestHandle: RequestHandleProtocol){
        self.rh = requestHandle
    }
    
    func handleRequest(request: RequestProtocol) {
        if request is AddMoneyRequest {
            print("要加薪, 项目经理审批!")
        } else {
            rh.handleRequest(request)
        }
    }
}

///  处理它所负责的请求。可访问它的后继者。
private class TLRequestHandle: RequestHandleProtocol {
    
    var rh: RequestHandleProtocol
    
    init(requestHandle: RequestHandleProtocol){
        self.rh = requestHandle
    }
    
    func handleRequest(request: RequestProtocol) {
        if request is LeaveRequest {
            print("要请假, 项目组长审批!")
        } else {
            rh.handleRequest(request)
        }
    }
    
}

// MARK: -

/// 请求协议
private protocol RequestProtocol {
    
}

/// DimissionRequest离职请求
private class DimissionRequest: RequestProtocol {
    
}

/// 加薪请求
private class AddMoneyRequest: RequestProtocol {
    
}

/// 请假请求
private class LeaveRequest: RequestProtocol {
    
}
```

测试

```swift
let hr = HRRequestHandle()
let pmRH = PMRequestHandle(requestHandle: hr)
let tl  = TLRequestHandle(requestHandle: pmRH)
// 向链上的具体处理者(ConcreteHandler)对象提交请求
// team leader处理离职请求
var request: RequestProtocol = DimissionRequest()
tl.handleRequest(request)
print("====")
// team leader处理加薪请求
request = AddMoneyRequest()
tl.handleRequest(request)
print("====")
// team leader处理请假请求
request = LeaveRequest()
tl.handleRequest(request)
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