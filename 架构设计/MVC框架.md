MVC全名是Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。

MVC被独特的发展起来用于映射传统的输入、处理和输出功能在一个逻辑的图形化用户界面的结构中。

# 1 简介

MVC开始是存在于桌面程序中的，M是指业务模型，V是指用户界面，C则是控制器，使用MVC的目的是将M和V的实现代码分离，从而使同一个程序可以使用不同的表现形式。

比如一批统计数据可以分别用柱状图、饼图来表示。C存在的目的则是确保M和V的同步，一旦M改变，V应该同步更新。

# 2 框架内容

MVC是一个框架模式，它强制性的使应用程序的输入、处理和输出分开。使用MVC应用程序被分成三个核心部件：模型、视图、控制器。它们各自处理自己的任务。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112724.png)

## 2.1 视图

视图是用户看到并与之交互的界面。

MVC好处是它能为应用程序处理很多不同的视图。在视图中其实没有真正的处理发生，不管这些数据是联机存储的还是一个雇员列表，作为视图来讲，它只是作为一种输出数据并允许用户操纵的方式。 

## 2.2 模型

模型表示企业数据和业务规则。

在MVC的三个部件中，模型拥有最多的处理任务。例如它可能用像EJBs和ColdFusion Components这样的构件对象来处理数据库，被模型返回的数据是中立的，就是说模型与数据格式无关，这样一个模型能为多个视图提供数据，由于应用于模型的代码只需写一次就可以被多个视图重用，所以减少了代码的重复性。[6] 

## 2.3 控制器

控制器接受用户的输入并调用模型和视图去完成用户的需求，所以当用户交互时，控制器本身不输出任何东西和做任何处理。它只是接收请求并决定调用哪个模型构件去处理请求，然后再确定用哪个视图来显示返回的数据。

# 3 架构实现

相关搭建项目环节就不介绍了，直接显示最核心的过程。

## 3.1 模型

模型在IOS开发过程中可以是数据库、服务器交互、实例等数据，这里模拟常用的网络交互。

```swift
//
//  Model.swift
//  MVC
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/27.
//  Copyright © 2015年 阳君. All rights reserved.
//

import Foundation

/// 模型层的协议
protocol ModelProtocol {
    
    func execute(model: Model)
    
}

/// 模型层
class Model {

    /// 相关数据，发送到服务器的数据以及从服务器接收的数据
    var data: Dictionary<String, AnyObject>?
    /// 回调代理
    var delegate: ModelProtocol?
    
    /// 向服务器发送数据
    ///
    /// - returns: void
    func sendRequest() {
        print("服务器接收数据:\(self.data)")
        if self.data == nil {
            self.data = Dictionary()
        }
        self.data!["qq"] = "937447974"
        print("服务器数据处理完毕，回调中...")
        self.delegate?.execute(self)
    }
    
}
```

定义了一个模型类Model以及网络回调协议ModelProtocol。当外部调用类遵循这种协议时即可回调数据。

## 3.2 控制器

控制器这里使用常用的ViewController做测试，然后通过模型发出网络请求，并处理回调数据。

```swift
//
//  ViewController.swift
//  MVC
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/27.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

class ViewController: UIViewController, ModelProtocol {

    override func viewDidLoad() {
        super.viewDidLoad()
        // 向服务器发送数据
        let model = Model()
        model.data = Dictionary()
        model.data!["name"] = "阳君"
        model.delegate = self // 当前类接收数据
        print("开始请求服务器")
        model.sendRequest()
    }
    
    func execute(model: Model) {
        print("UI接收数据：\(model.data)")
    }

}
```

&#160;

----------

# 其他

## 源代码

[Framework](https://github.com/937447974/Framework)

## 参考资料

[MVC框架](http://baike.baidu.com/link?url=QKxbbSFJi6raVaMwEWfu_ne5mnfU07wsxCIK1rim12HrBUJXaM3jbkHCnW1hZNdaEKEhJrAfPGqdDVbrJXVy4Ug5hoyS94PQhCF2xj_gfNZLJYqkmgBtLOBcragA-zqR)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-27 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog