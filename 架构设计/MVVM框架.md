MVVM是Model-View-ViewModel的简写。 

微软的WPF带来了新的技术体验，如Silverlight、音频、视频、3D、动画……，这导致了软件UI层更加细节化、可定制化。同时，在技术层面，WPF也带来了 诸如Binding、Dependency Property、Routed Events、Command、DataTemplate、ControlTemplate等新特性。

MVVM（Model-View-ViewModel）框架的由来便是MVP（Model-View-Presenter）模式与WPF结合的应用方式时发展演变过来的一种新型架构框架。它立足于原有MVP框架并且把WPF的新特性糅合进去，以应对客户日益复杂的需求变化。 
 
#1 简介

WPF的数据绑定与Presentation Model相结合是非常好的做法,使得开发人员可以将View和逻辑分离出来,但这种数据绑定技术非常简单实用，也是WPF所特有的，所以我们又称之为Model-View-ViewModel(MVVM)。 

这种模式跟经典的MVP（Model-View-Presenter）模式很相似，除了你需要一个为View量身定制的model，这个model就是ViewModel。ViewModel包含所有由UI特定的接口和属性，并由一个 ViewModel 的视图的绑定属性，并可获得二者之间的松散耦合，所以需要在ViewModel 直接更新视图中编写相应代码。 

数据绑定系统还支持提供了标准化的方式传输到视图的验证错误的输入的验证。 
 

#2 优点

低耦合。视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定到不同的”View”上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。

可重用性。你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑。

独立开发。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用Expression Blend可以很容易设计界面并生成xaml代码。

可测试。界面素来是比较难于测试的，而现在测试可以针对ViewModel来写。 
 
#3 MVVM结构图 
 
![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112802.png)

#4 架构实现。

#4.1 搭建项目

苹果也考虑了这种MVVM框架的实现。MVVM的核心就是VM，也就是视图和Model的双向绑定。可以运用Objec控件完成双向绑定。

下面省略了搭建项目的其他步骤，只显示双向绑定的过程。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015112803.gif)

##4.2 Model层

Model层和MVP框架的模型层使用同样的代码结构。

```swift
//
//  YJModel.swift
//  MVP
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/28.
//  Copyright © 2015年 阳君. All rights reserved.
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
        print("Model收到服务器发回的数据，通知ViewModel层")
        print("Model End============")
        self.delegate?.execute(self)
    }
    
}
```

定义了一个模型类Model以及网络回调协议ModelProtocol。当外部调用类遵循这种协议时即可回调数据。

##4.3 ViewModel层

ViewModel优化了MVP中Presenter层，让View层彻底解放，只关心界面的展示。服务器返回的数据，在这一层就处理完毕了。

```swift
//
//  YJViewModel.swift
//  MVVM
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/28.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// ViewModel层
class YJViewModel: NSObject, YJModelProtocol {
    
    /// 姓名
    @IBOutlet weak var nameLabel: UILabel!
    
    func viewDidLoad() {
        // 开始数据准备
        print("\nViewModel层收到View层指令")
        print("ViewModel Begin++++++++++++")
        // 向服务器发送数据
        let model = YJModel()
        model.data = Dictionary()
        model.data!["name"] = "阳君"
        model.delegate = self // 当前类接收数据
        print("开始请求Model层获取数据")
        model.sendRequest()
    }
    
    func execute(model: YJModel) {
        print("\nViewModel层收到Model层数据：\(model.data)")
        self.nameLabel.text = model.data?["name"] as? String
        print("ViewModel End++++++++++++")
    }
    
}
```

##4.4 View层

View层就是ViewController了。

```swift
//
//  YJViewController.swift
//  MVVM
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/28.
//  Copyright © 2015年 阳君. All rights reserved.
//

import UIKit

/// View层
class YJViewController: UIViewController {

    /// ViewModel层
    @IBOutlet var viewModel: YJViewModel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        print("View层初始化UI")
        print("View通知ViewModel加载数据")
        // 视图加载
        self.viewModel.viewDidLoad()
    }
    
}
```

从代码中可以看出业务逻辑已经完全扔给ViewModel做处理了。

##4.4 测试

运行项目后可看见如下输出效果,会发现层与层之间的关系非常清晰，代码的可维护度非常高。

```swift
View层初始化UI
View通知ViewModel加载数据

ViewModel层收到View层指令
ViewModel Begin++++++++++++
开始请求Model层获取数据

Model Begin============
Model收到通知开始请求服务器
服务器接收数据:Optional(["name": 阳君])
服务器数据处理完毕，回发中...
Model收到服务器发回的数发中...
Model收到服务器发回的数\346\215据，通知ViewModel层
Model End============

ViewModel层收到Model层数据：Optional(["qq": 937447974, "name": 阳君])
ViewModel End++++++++++++
```

&#160;

----------

#其他

##源代码

[Framework](https://github.com/937447974/Framework)

##参考资料

[MVVM](http://baike.baidu.com/view/3507915.htm)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-28 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog