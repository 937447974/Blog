在Swift开发过程中，我们会遇到以下情况：判断某个实例是那个类生成的；将子类转换为父类；想让一个变量可以为任何类型（值对象、引用对象、方法）。。。

Swift也能处理这些情况，需要使用的关键字：类型判断is、类型转换as、属性声明AnyObject和Any。

在介绍这四个关键字的使用前，先构建类MediaItem、Movie和Song。

```swift
class MediaItem {
    
}
    
class Movie: MediaItem {

}
    
class Song: MediaItem {

}
```

其中Movie和Song都继承MediaItem。
&#160;

#Is

is主要用于类型判断，如我们判断某个实例是那个类的子类，或是那个类生成的。

```swift
let array = [Song(), Movie()]
    
// is测试，类型判断
for item in array {
    if item is Movie {
        print("Movie构建")
    } else if item is Song {
        print("Song构建")
    }
}
```

&#160;

#As

as主要用于类型转换，可将一个类转换为另一个类。as后可以跟'?'或'!'。

- as?：预转换，转换失败时，返回nil。
- as!：强转换，转换失败时，程序崩溃。

```swift
for item in array {
    if let movie = item as? Movie {
        print("可转换为Movie: '\(movie)'")
    } else if let song = item as? Song {
        print("可转换为Song: '\(song)'")
    }
    // 强转换，失败时，程序崩溃
    let movie = item as! Movie
}
```

&#160;

#AnyObject

使用AnyObject声明的常量（变量）可以是值对象或引用对象。

```swift
let someObjects: [AnyObject] = [Movie(), 1, "33"]
```

&#160;

#Any

Any和AnyObject具有相同的特性，只是Any还可以代表方法和闭包。

```swift
var things = [Any]()
things.append(0) // 值类型
things.append(Movie()) // 引用类型
things.append({ (name: String) -> String in "Hello, \(name)" }) // 闭包
```

&#160;

----------

#其他

##参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-1 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Type Casting总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j