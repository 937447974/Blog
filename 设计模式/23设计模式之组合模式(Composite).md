# 1 概述

Composite属于结构型模式中的一种，将对象组合成树形结构以表示"部分-整体"的层次结构。Composite使得用户对单个对象和组合对象的使用具有一致性。

# 2 适用性

1. 你想表示对象的部分-整体层次结构。
2. 你希望用户忽略组合对象与单个对象的不同，用户将统一地使用组合结构中的所有对象。

# 3 参与者

1. **Component**：为组合中的对象声明接口。在适当的情况下，实现所有类共有接口的缺省行为。声明一个接口用于访问和管理Component的子组件。(可选)在递归结构中定义一个接口，用于访问一个父部件，并在合适的情况下实现它。
2. **Leaf**：在组合中表示叶节点对象，叶节点没有子节点。在组合中定义节点对象的行为。
3. **Composite**：定义有子部件的那些部件的行为。存储子部件。在Component接口中实现与子部件有关的操作。
4. **Client**：通过Component接口操纵组合部件的对象。

# 4 类图

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112708.png)

# 5 代码实现

```swift
import Cocoa

/// Employer为组合中的对象声明协议
private class Employer {
    
    /// 员工姓名
    var name:String = ""
    /// 下属员工列表
    var employers:Array<Employer> = Array()
    
    // MARK: 打印
    func printInfo() {
        print(name);
    }
    
    // MARK: 增加
    func add(employer:Employer){
        
    }
    
    // MARK: 删除
    func delete(employer:Employer){
        
    }
    
    // MARK: 比较是否相等
    func equal(employer:Employer) ->Bool {
        if employer.name == self.name {
            return true
        }
        return false
    }
    
}

/// Programmer在组合中表示叶节点对象，叶节点没有子节点
private class Programmer: Employer {
    
    init(name:String){
        super.init()
        super.name = name
    }
    
}

/// ProjectAssistant在组合中表示叶节点对象，叶节点没有子节点
private class ProjectAssistant: Employer {
    
    init(name:String){
        super.init()
        super.name = name
    }
    
}

/// ProjectManager定义有子部件的那些部件的行为
private class ProjectManager: Employer {
    
    init(name:String){
        super.init()
        super.name = name
        super.employers = []
    }
    
    override func add(employer: Employer) {
        super.employers.append(employer)
    }
    
    override func delete(employer: Employer) {
        for(var i = 0; i < super.employers.count; i++) {
            if employer.equal(super.employers[i]) {
                super.employers.removeAtIndex(i)
                return
            }
        }
    }
    
}
```

```swift
let pm = ProjectManager(name: "项目经理")
let pa = ProjectAssistant(name: "项目经理")
let pm1 = Programmer(name: "程序员一")
let pm2 = Programmer(name: "程序员二")
// Client:通过Component协议操纵组合部件的对象。
pm.add(pa)
pm.add(pm1)
pm.add(pm2)
// 删除
pm.delete(pm2)
for item in pm.employers {
    item.printInfo()
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