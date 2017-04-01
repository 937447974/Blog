#1 创建型

##1.1 [Factory Method（工厂方法）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之工厂方法(FactoryMethod).md)

定义一个用于创建对象的接口，让子类决定实例化哪一个类，工厂模式使一个类的实例化延迟到其子类。

NSObject 的 alloc 方法后跟的 init 其实就是一个工厂方法，我们可以通过任何 class 执行alloc方法后，再调用 init 方法即可生成任何 class 的实例对象。

##1.2 [Abstract Factory（抽象工厂）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之抽象工厂(Abstract%20Factory).md)

提给一个创建一系列或相关依赖对象的接口，而无需指定它们具体的类。

NSURLSession 创建 NSURLSessionDataTask、NSURLSessionUploadTask 和 NSURLSessionDownloadTask 等对象就是抽象工厂模式。通过一个仓库生产多个不同的实例。

抽象工厂和工厂方法的本质区别就是抽象工厂是多个工厂多个接口生产多个产品。

##1.3 [Builder（建造者）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之建造者模式(Builder).md)

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

##1.4 [Prototype（原型）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之原型模式(Prototype).md)

用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。

NSObject 中的 `copy` 则是原型模式，通过拷贝自身达到创建对象。

##1.5 [Singleton（单例）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之单例模式(Singleton).md)

保证一个类仅有一个实例，并提供一个访问它的全局访问点。

[NSUserDefaults standardUserDefaults] 则是单例模式，整个app中唯一实例。

#2 结构型

##2.1 [Adapter Class/Object（适配器）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之适配器(Adapter).md)

将一个类的接口转化成客户希望的另外一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

##2.2 [Facade（外观）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之外观模式(Facade).md) 

为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。

##2.3 [Bridge（桥接）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之桥接模式(Bridge).md)

将抽象部分与它的实现部分分离，使它们都可以独立地变化。

##2.4 [Composite（组合）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之组合模式(Composite).md)

将对象组合成树形结构以表示‘部分-整体’的层次结构，组合模式使得用户对单个对象和组合对象的使用具有一致性。

##2.5 [Decorator（装饰）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之装饰模式(Decorator).md)

动态地给一个对象添加一些额外的职责。就增加功能来说，装饰模式相比子类更加灵活。

##2.6 [Flyweight（享元）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之享元模式(Flyweight).md)

运用共享技术有效地支持大量细粒度的对象。

[NSUserDefaults standardUserDefaults] 内存储数据的方式是一个享元模式，通过它内部的 NSMutableDictionary 我们可以存储任何我们想存储的基本类型数据。

##2.7 [Proxy（代理）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之代理模式(Proxy).md)

为其他对象提供一种代理以控制对这个对象的访问。

UITableViewDataSource 则是代理模式，通过它的代理回调我们控制了 UITableView 的相关行为。

#3 行为型

##3.1 梯队

###3.1.1 [Observer（观察者）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之观察者模式(Observer).md) 

定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

KVO 则是一种观察模式，我们可以动态监听对象属性值的变化。

###3.1.2 [Template Method（模板方法）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之模板方法(TemplateMethod).md)

定义一个操作的算法骨架，而将一些步骤延迟到子类中，模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

init 则是模板方法。NSObject 定义了 init 方法，我们可子类重写这个方法初始化自己的相关操作。

###3.1.3 [Chain of Responsibility（责任链）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之责任链模式(COR).md)

使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止。

调用方法，其对象 isa 的流向则是责任链模式，简单点说就是当前类能处理则处理，不能处理就交给其父类处理。

###3.1.4 [Command（命令）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之命令模式(Command).md)

将一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数化；可以对请求排队或记录请求日志，以及支持可撤销的操作。

###3.1.5 [State（状态）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之状态模式(State).md)

允许一个对象在其内部状态改变时改变它的行为，让对象看起来似乎修改了它的类。

2梯队

###3.1.1 [Strategy（策略）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之策略模式(Strategy).md)

定义一系列的算法，把它们一个个封装起来，并且使它们可相互替换。本模式使得算法可独立于使用它的客户而变化。

###3.1.2 [Interpreter（解释器）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之解释器模式(Interpreter).md)

给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子。

NSString 的 boolValue 则是内嵌的解释器模式，将字符串解释成 bool 值。

###3.1.3 [Iterator（迭代器）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之迭代器模式(Iterator).md)

提供一种方法顺序访问一个聚合对象中的各个元素，而又不需要暴露该对象的内部表示。

forin 遍历数组则是迭代器模式。

###3.1.4 [Mediator（中介者）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之中介者模式(Mediator).md)

用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

###3.1.5 [Memento（备忘录）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之备忘录模式(Memento).md)

在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可将该对象恢复到原先保存的状态。

###3.1.6 [Visitor（访问者）](https://github.com/937447974/Blog/blob/master/架构设计/23设计模式之工厂方法(FactoryMethod).md)

一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

&#160;

----------

#Appendix

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-26 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974