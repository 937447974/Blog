方法是与特定类型关联的函数。在类、结构体和枚举中都可以定义方法。方法可以是实例方法，也可以是类型方法。
&#160;

# 实例方法

实例方法是属于某一特定类、结构体或枚举的函数。它们提供访问和修改实例属性的方法，或者提供与实例相关的功能。实例方法和函数的功能相同，具有完全相同的语法。

在实例方法的大括号内，可以访问局部变量和全局变量。实例方法同样也可以调用其他实例方法。

## 局部变量和全局变量

```swift
class Counter {
    var count: Int = 0
    func incrementBy(amount: Int, numberOfTimes: Int) {
        count += amount * numberOfTimes
    }
}
let counter = Counter()
counter.incrementBy(5, numberOfTimes: 3)
print("\(counter.count)") // 15
```

如上所示：类Counter中有一个变量count,和一个实例方法incrementBy。在incrementBy方法内可以访问全局变量count。

## 修改外部参数名称

如上面的例子，在外部调用方法时使用`incrementBy(5, numberOfTimes: 3)`,如果你觉得这样太麻烦，我们还可以省略numberOfTimes这个名称，只需参数名前加下划线。即修改方法体为`func incrementBy(amount: Int, _ numberOfTimes: Int)`,此时的外部访问变为`counter.incrementBy(5, 3)`。

## Self属性

在Swift中也可以使用self访问全局属性或全局方法。

```Swift
struct Point {
    var x = 0.0, y = 0.0
    func isToTheRightOfX(x: Double) -> Bool {
        // 这里有内部和外部属性
        return self.x > x
    }
}
let somePoint = Point(x: 4.0, y: 5.0)
print("\(somePoint.isToTheRightOfX(1.0))") // true
```

## 在实例方法中修改值类型

在swift中，不但可以给类添加方法，还可以在结构体和枚举中添加方法。

```Swift
struct Point {
    var x = 0.0, y = 0.0
     mutating func moveByX(deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveByX(2.0, y: 3.0)
print("(\(somePoint.x), \(somePoint.y))") // (3.0, 4.0)
let fixedPoint = Point(x: 3.0, y: 3.0)
// 结构体是值对象，使用let常量后，无法修改内部值
fixedPoint.moveByX(2.0, y: 3.0) // 抛错
```

由于结构体和枚举是值类型，默认情况下，实例方法中是不可以修改值类型的属性。我们只可以在外部修改值类型中的属性。如果你想在实例中修改值类型的属性，只需在方法名前加mutating关键字。

## 在实例方法中修改结构体或枚举

在结构体和枚举内，self代表当前结构体或枚举，我们还可以通过mutating修改当前结构体或枚举。

```Swift
// 结构体测试
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveByX(deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}

// 枚举测试
enum TriStateSwitch {
    case Off, Low, High
    mutating func next() {
        switch self {
        case Off:
            self = Low
        case Low:
            self = High
        case High:
            self = Off
        }
    }
}

var ovenLight = TriStateSwitch.Low
print("\(ovenLight.next())") // High
print("\(ovenLight.next())") // Off
```

如上所示，在实例方法中就可修改self。

&#160;

# 类型方法

在方法中我们可以不初始化类就调用类的内部方法，这就是类型方法。

```Swift
class SomeClass {
    class func someTypeMethod() {
        // type method implementation goes here
    }
}

SomeClass.someTypeMethod()
```

如上，我们只需在方法名前添加class关键字，就可直接使用类名加方法名调用someTypeMethod方法。

接下来为大家演示怎么直接使用结构体名加方法名。使用到的关键字为static

```Swift
// 结构体
struct LevelTracker {
    // static修改属性，方法体要修改static属性，方法前要使用static
    static var highestUnlockedLevel = 1
    static func levelIsUnlocked(level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }
}

print("\(LevelTracker.levelIsUnlocked(2))") // false
```

&#160;

----------

# 其他

## 参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-10-28 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Methods总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j