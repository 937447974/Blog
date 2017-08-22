玩过其他开发语言的小伙伴都知道继承，也就是子类继承父类的特性。这在开发过程中非常有用，可以节省大量工作量。
&#160;

# 声明基类

下面我们声明一个常见的基类Base,有两个属性（count、description）和一个方法（inherited），其中description是只读的。

```
/// 基类
class Base {
    var count = 0.0
    var description: String {
        return "count:\(count)"
    }
    
    // MARK: 可继承
    func inherited() {
        
    }
   
}
```

&#160;

# 子类化

子类继承基类很简单，声明如下，子类和父类间用“：”隔开，如果有多个父类，父类间用“，”隔开。

```Swift
/// 子类
class Subclass: Base {
        
}
```

&#160;

# 重写

在子类中可以实现父类的所有特性，但是有的时候我们想扩展属性或方法。这个时候就需要用到关键字Overriding。

下面让子类重写父类的属性和方法。

```Swift
class Subclass: Base {
    
    // 继承的属性和方法前都有override
    override var count:Double {
        didSet {
            print("\(__FUNCTION__)")
        }
    }
    
    override var description: String {
        return "\(__FUNCTION__)" + super.description
    }
    
    override func inherited() {
        print("\(__FUNCTION__)")
    }
}
```

可以在属性和方法前使用override，表示这个属性和方法是继承了父类。如果想调用父类的属性或方法，只需要使用super后面跟你想使用的属性和方法。你可以理解为self代表当前类，super代表父类。
&#160;

# 防止重写

有的时候我们不希望其他人通过继承改写我们的特性，希望它是不可重写的。这里可以使用关键字final，如下所示：

- final var/let：防止常量或变量被重写；
- final func：防止实例方法被重写；
- final class func：防止类型方法被重写；
- final subscript：防止下标方法被重写；
- final class：防止当前类被继承。

这里以实例方法举例，在Base中添加如下方法：

```Swift
final func preventing() {
    // 如果不想子类继承，可在类、属性或方法前添加final
}   
```

则子类Subclass不可重写此方法。
&#160;

----------

# 其他

## 参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-10-28 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Inheritance总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j