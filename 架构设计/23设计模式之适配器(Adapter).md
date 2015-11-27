一、概述

       Adapter属于结构型模式中的一种，将一个类的接口转换成客户希望的另外一个接口。Adapter模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

二、适用性

       1. 你想使用一个已经存在的类，而它的接口不符合你的需求。
       2. 你想创建一个可以复用的类，该类可以与其他不相关的类或不可预见的类（即那些接口可能不一定兼容的类）协同工作。
       3. 你想使用一些已经存在的子类，但是不可能对每一个都进行子类化以匹配它们的接口。对象适配器可以适配它的父类接口。（仅适用于对象Adapter）

三、参与者

       1. Target:定义Client使用的与特定领域相关的接口。
       2. Client:与符合Target接口的对象协同。
       3. Adaptee:定义一个已经存在的接口，这个接口需要适配。
       4. Adapter:对Adaptee的接口与Target接口进行适配。

四、类图 

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

五、代码实现

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

&#160;

----------

#其他

##源代码

[Framework](https://github.com/937447974/Framework)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-16 | 项目完成、博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/BlogÍ