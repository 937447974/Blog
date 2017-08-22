

实例的初始化是准备一个类、结构体或枚举的实例以便使用的过程。初始化包括设置一个实例存储属性的初始值，以及其他相关设置。

一个类、结构体或枚举能定义一个初始化方法来设置它的特性，用来确保它的所有属性都是有效的初始值。

通过调用类、结构或枚举提供的初始化方法可以执行实例的初始化过程。

>构造初始化类、结构体和枚举的初始化方法所使用到的关键字是init。

&#160;

# 初始化

## 初始化结构体

下面我们定义一个结构体Size，并添加两个属性wdith和height。

```Swift
struct Size {
    /// 宽
    var width = 0.0
    /// 高
    var height = 0.0
}
    
```

使用结构体自带的初始化方法实例化结构体：

```Swift
// 默认初始化，使用的是init()构造方法
var size = Size()
// 结构体自带根据成员初始化结构体的功能
size = Size(width: 10, height: 10)
```

接下来我们使用自定义的初始化功能初始化结构体，这里我们创建一个结构体Point，并添加两个属性x、y以及相关初始化方法。

```Swift
struct Point {
    
    var x = 0.0, y = 0.0
    
    // MARK: 默认初始化
    init() {
        
    }
    
    // MARK: 通过成员初始化结构体
    init(x:Double, y:Double) {
        self.x = x
        self.y = y
    }
    
}

// 自定义初始化
var point = Point() // 调用inin()方法
point = Point(x: 10, y: 10) // 调用init(x: Double, y:Double)方法

```

这就是仿照系统自带的方法实现。但是当你在结构体自定义初始化方法时，则无法在外部使用系统自带的初始化方法。

## 初始化枚举

枚举和结构体一样也有一些系统自带的初始化方法，和结构体不同的是，你在枚举中自定义方法后，还是可以使用系统自带的初始化方法。下面我们以一个CompassPoint枚举举例，它的内部是String类型。

```Swift
enum CompassPoint:String {
    
    case North, South, East, West
    
    init(symbol: String) {
        switch symbol {
        case "North":
            self = .North
        case "South":
            self = .South
        case "East":
            self = .East
        default:
            self = .West
        }
    }
    
}
    
// 直接取值
var compassPoint = CompassPoint.West
// 通过原始值初始化
compassPoint = CompassPoint(rawValue: "North")!
// 通过自定义的初始化方法
compassPoint = CompassPoint(symbol: "North")
```

在这里为大家介绍了三种初始化方法：直接取值、通过原始值和通过自定义的方法获取枚举。

## 初始化类

在Swift中，类是一个特殊的类型，它是引用类型，并且可继承。这里定义两个类：基类BaseClass和子类SubClass。

```swift
class BaseClass {
    
    var name: String? // 可选属性类型，可能为String或nil
    
    init() {
        print("Food:init()")
    }
    
    convenience init(name: String) {
        self.init()
        self.name = name
    }
    
    // MARK: - 省略外部参数
    convenience init(_ subName: String) {
        self.init(name: subName)
    }
    
}
```

### 子类调用父类初始化方法

在子类的初始化方法中想调用父类的初始化方法，只需使用关键字super。

```Swift
class SubClass: BaseClass {
    
    override init() {
        super.init() // 实现父类的init()方法
    }
    
}
```

### 默认初始化

类和结构体一样也有默认初始化，不同的是它只有一个默认初始化方法。

```swift
var user = SubClass()
```

### 自定义初始化

有的时候我们希望定制一些初始化方法方便外部调用，这里我们定制了初始化name的方法`init(name: String)`。

正如大家看见的在这个方法中我们使用了关键字convenience。convenience的作用就是当我们在一个初始化方法体内想调用另一个初始化方法体，则需要在func前加convenience。如我们在`init(name: String)`中使用了初始化方法`self.init()`

调用如下:

```swift
// 通过属性初始化
user = SubClass(name: "阳君")
```

### 省略外部参数

在外部使用自定义的初始化方法时，需要使用参数名，如果你不想使用参数名，也可以在初始化的方法中参数名前加`_`,如

```Swift
convenience init(_ subName: String) {
    self.init(name: subName)
}

// 使用init(_ name: String)初始化
user = SubClass("阳君")

```

&#160;

# 初始化失败

在开发过程中，外部使用初始化方法时，由于传入的参数不符合规范，我们需要返回nil，也就是初始化失败。类、枚举和结构体都可以初始化失败。构造可返回nil的初始化方法很简单，只需要在nil后添加“？”。

## 结构体初始化失败

```swift
struct Animal {

    let species: String
    
    init?(species: String) {
        if species.isEmpty { return nil }
        self.species = species
    }
    
}

let someCreature = Animal(species: "Giraffe")
// 运用可选链判断
if let giraffe = someCreature {
    print("\(giraffe.species)") // Giraffe
}
```

使用Animal的init?(species: String)方法可能返回nil，也就是说someCreature为Animal或nil，接下来我们用了可选链的判断方式。当giraff不为nil时输出species属性。

## 初始化枚举失败

```swift
enum TemperatureUnit:Character {
    
    case Kelvin = "K", Celsius = "C", Fahrenheit = "F"
    
    init?(symbol: Character) {
        switch symbol {
        case "K":
            self = .Kelvin
        case "C":
            self = .Celsius
        case "F":
            self = .Fahrenheit
        default:
            return nil
        }
    }
    
}
// 通过自定义方法初始化
var unit = TemperatureUnit(symbol: "F")
print(unit) // Fahrenheit
unit = TemperatureUnit(symbol: "X")
print(unit) // nil
    
// 通过原始值初始化
unit = TemperatureUnit(rawValue: "F")
print(unit) // Fahrenheit
unit = TemperatureUnit(rawValue: "X")
print(unit) // nil

```

使用枚举初始化失败有两种方式：一种是自定义的可返回nil方法；一种是使用原始值获取枚举。

## 类初始化失败

```swift
class Product {
    
    var name: String?
    
    init() {}
    
    init?(name: String) {
        self.name = name
        if name.isEmpty {
            return nil
        }
    }
}

// 可选链操作，当bowTie为真时，执行内部代码
if let qq = Product(name: "937447974") {
    print(qq.name) // 937447974
}
```

&#160;

# 必须初始化

有的时候我们会碰到这样的场景，如父类提供了初始化方法，当子类继承时，希望子类也强制性的初始化这个方法。这里就用到了关键字required。

```swift
class SomeClass {
    
    required init() {
        // required：子类要调用此方法，必须继承实现
    }
    
}
    
class SomeSubclass: SomeClass {
    
    required init() {
        
    }
    
    init(required:String) {
        super.init()
        // 这里调用父类的init()方法，当前类必须实现init()方法
    }
}
```

&#160;

# 使用闭包或函数设置默认属性

在类中我们可以设置属性的默认值，我们还可以通过闭包或函数设置属性的默认值。

```
class SomeClass {
    
    let someProperty: String = {
        // create a default value for someProperty inside this closure
        // someValue must be of the same type as SomeType
        return "someValue"
    }()
    
}
    
let c = SomeClass()
print(c.someProperty) // someValue
```

&#160;

----------

# 其他

## 参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-10-31 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Initialization总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j