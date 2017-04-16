MVP的全称为Model-View-Presenter，Model提供数据，View负责显示，Controller/Presenter负责逻辑的处理。 

MVP与MVC有着一个重大的区别：在MVP中View并不直接使用Model，它们之间的通信是通过Presenter (MVC中的Controller)来进行的，所有的交互都发生在Presenter内部，而在MVC中View会直接从Model中读取数据而不是通过 Controller。 

# 1 简介

MVP 是从经典的模式MVC演变而来，它们的基本思想有相通的地方：Controller/Presenter负责逻辑的处理，Model提供数据，View负责显示。 

作为一种新的模式，MVP与MVC有着一个重大的区别：在MVP中View并不直接使用Model，它们之间的通信是通过Presenter (MVC中的Controller)来进行的，所有的交互都发生在Presenter内部，而在MVC中View会从直接Model中读取数据而不是通过 Controller。 

# 2 优缺点

## 2.1 优点

1. 模型与视图完全分离，我们可以修改视图而不影响模型
2. 可以更高效地使用模型，因为所有的交互都发生在一个地方——Presenter内部
3. 我们可以将一个Presenter用于多个视图，而不需要改变Presenter的逻辑。这个特性非常的有用，因为视图的变化总是比模型的变化频繁。
4. 如果我们把逻辑放在Presenter中，那么我们就可以脱离用户接口来测试这些逻辑（单元测试）

## 2.2 缺点

1. 由于对视图的渲染放在了Presenter中，所以视图和Presenter的交互会过于频繁。
2. 如果Presenter过多地渲染了视图，往往会使得它与特定的视图的联系过于紧密。一旦视图需要变更，那么Presenter也需要变更了。 
 
## 3 MVP结构图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112801.png)

# 4 架构实现

相关搭建项目环节就不介绍了，直接显示最核心的过程。

## 4.1 搭建项目

苹果也考虑了这种MVP框架的实现。MVP的核心就是Presenter，也就是视图和Model的双向绑定。可以运用Objec控件完成双向绑定。

下面省略了搭建项目的其他步骤，只显示双向绑定的过程。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112803.gif)

## 4.1 Model层

Model层和MVC框架的模型层使用同样的代码结构。

```swift
//
//  YJModel.swift
//  MVP
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/15.
//  Copyright © 2016年 阳君. All rights reserved.
//

import Foundation

/// 模型层的协议
protocol YJModelProtocol {
    
    func execute(model: YJModel)
    
}

/// model层，处理与服务器的响应
class YJModel: NSObject {
    
    /// 相关数据，发送到服务器的数据以及从服务器接收的数据
    var data: Dictionary<String, AnyObject>?
    /// 回调代理
    var delegate: YJModelProtocol?
    
    /// 向服务器发送数据
    ///
    /// - returns: void
    func sendRequest() {
        print("\nModel Begin============")
        print("Model收到通知开始请求服务器")
        print("服务器接收数据:\(self.data)")
        if self.data == nil {
            self.data = Dictionary()
        }
        self.data!["qq"] = "937447974"
        print("服务器数据处理完毕，回发中...")
        print("Model收到服务器发回的数据，通知上一层")
        print("Model End============")
        self.delegate?.execute(self)
    }
    
}
```

定义了一个模型类Model以及网络回调协议ModelProtocol。当外部调用类遵循这种协议时即可回调数据。

## 4.2 Presenter层

Presenter与具体的View是直接关联的，从而使得在变更View时候可以变更Presenter。

```swift
//
//  YJPresenter.swift
//  MVP
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/15.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit

/// Presenter层
class YJPresenter: NSObject, YJModelProtocol {
    
    /// 姓名
    @IBOutlet weak var nameLabel: UILabel!
    
    func viewDidLoad() {
        // 开始数据准备
        print("\nPresenter层收到View层指令")
        print("Presenter Begin++++++++++++")
        // 向服务器发送数据
        let model = YJModel()
        model.data = Dictionary()
        model.data!["name"] = "阳君"
        model.delegate = self // 当前类接收数据
        print("开始请求Model层获取数据")
        model.sendRequest()
    }
    
    func execute(model: YJModel) {
        print("\nPresenter层收到Model层数据：\(model.data)")
        self.nameLabel.text = model.data?["name"] as? String
        print("Presenter End++++++++++++")
    }
    
}
```

在Presenter层定义了YJPresenterProtocol协议，这是Presenter和View层沟通的通道。在Presenter层我们实现了和Model层的交互，这样就解放了View层的代码量。

## 4.3 View层

View层就是ViewController了，这里的ViewController只控制UI界面的交互，至于和服务器之间的交互交给Presenter层。

```swift
//
//  YJViewController.swift
//  MVP
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/15.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit

/// View层
class YJViewController: UIViewController {
    
    /// Presenter层
    @IBOutlet var presenter: YJPresenter!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        print("View层初始化UI")
        print("View通知Presenter加载数据")
        // 视图加载
        self.presenter.viewDidLoad()
    }
    
}
```

## 4.4 测试

运行项目后可看见如下输出效果,会发现层与层之间的关系非常清晰，代码的可维护度非常高。

```swift
View层初始化UI
View通知Presenter加载数据

Presenter层收到View层知Presenter加载数据

Presenter层收到View层\346指令
Presenter Begin++++++++++++
开始请求Model层获取数据

Model Begin============
Model收到通知开始请求服务器
服务器接收数据:Optional(["name": 阳君])
服务器数据处理完毕，回发中...
Model收到服务器发回的数据，通知上一层
Model End============

Presenter层收到Model层数据：Optional(["qq": 937447974, "name": 阳君])
Presenter End++++++++++++
```

&#160;

----------

# 其他

## 源代码

[Framework](https://github.com/937447974/Framework)

## 参考资料

[MVP](http://baike.baidu.com/link?url=tfAlBj2kb5Y2rfRpPLd3EwgNUGWSoQcQRc72v9FF7t9sSGCmtdtCZ5tS1m-bxPSWK0dVLKls3GUnMX7vWlYWbr-VwvfYBqdt3JGDBEBRr8O)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-28 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog