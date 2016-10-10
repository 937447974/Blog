在Swift中，类、结构体和枚举都是支持下标语法的。

什么是下标语法？使用过数组、字典的朋友都见过array[index]。通过这样的方式可以设置数据和取数，会很方便也很简洁。你可以给一个类定义多个下标，也可以在一个下标中定义一个或多个参数。

下标的关键字是subscript，常用格式如下：

```Swift
subscript(index: Int) -> Int {
    get {
        // return an appropriate subscript value here
    }
    set(newValue) {
        // perform a suitable setting action here
    }
}
```

下面我们以数组为例，给大家介绍下标的创建和使用。

```Swift
/// array结构体
struct TestArray {
    
    /// 内部数组
    var array = Array<Int>()
    
    // MARK: 下标使用
    subscript(index: Int) -> Int {
        get {
            assert(index < array.count, "下标越界")
            return array[index]
        }
        set {
            while array.count <= index {
                array.append(0)
            }
            array[index] = newValue
        }
    }
}
    
var array = TestArray()
array[3] = 4; // 通过下标设置值
print("\(array[3])") // 4
print("\(array[4])") // 程序停止
```

&#160;

----------

##其他

###参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

###文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-10-30 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Subscripts总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j