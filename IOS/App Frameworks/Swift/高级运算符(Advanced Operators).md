# 按位运算符

## ~

1变0，0变1。

![~](http://img.blog.csdn.net/20151103110826597)

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // equals 11110000
```

## $

全1得1，其他为0

![$](http://img.blog.csdn.net/20151103110852494)

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits  // equals 00111100
```

## |

有1得1，其他为0

![|](http://img.blog.csdn.net/20151103110944324)

```Swift
let someBits: UInt8 = 0b10110010
let moreBits: UInt8 = 0b01011110
let combinedbits = someBits | moreBits  // equals 11111110
```

## ^

相异为1，相同为0

![^](http://img.blog.csdn.net/20151103111014304)

```swift
let firstBits: UInt8 = 0b00010100
let otherBits: UInt8 = 0b00000101
let outputBits = firstBits ^ otherBits  // equals 00010001
```

## <<和>>

<<整体左移，右边填0；

\>> 整体右移，左边填0。

![<<和>>](http://img.blog.csdn.net/20151103111041263)

```swift
let shiftBits: UInt8 = 4   // 00000100 in binary
shiftBits << 1             // 00001000
shiftBits << 2             // 00010000
shiftBits << 5             // 10000000
shiftBits << 6             // 00000000
shiftBits >> 2             // 00000001
```

&#160;

# 算子函数

上面讲解的都是简单的运算符，下面的是为对象添加运算符，使之可计算。

## 符号在中间

```Swift
struct Vector2D {
    var x = 0.0, y = 0.0
}

func + (left: Vector2D, right: Vector2D) -> Vector2D {
    return Vector2D(x: left.x + right.x, y: left.y + right.y)
}

let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector is a Vector2D instance with values of (5.0, 5.0)
```

## 前缀和后缀

前缀关键字prefix
后缀关键字postfix

```swift
prefix func - (vector: Vector2D) -> Vector2D {
    return Vector2D(x: -vector.x, y: -vector.y)
}

let positive = Vector2D(x: 3.0, y: 4.0)
let negative = -positive
// negative is a Vector2D instance with values of (-3.0, -4.0)
let alsoPositive = -negative
// alsoPositive is a Vector2D instance with values of (3.0, 4.0)

```

## 复合赋值运算符

这里用+=举例。

```
func += (inout left: Vector2D, right: Vector2D) {
    left = left + right
}

var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// original now has values of (4.0, 6.0)
```

&#160;

----------

# 其他

## 参考资料

 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-1 | 根据 [The Swift Programming Language (Swift 2.1)](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)中的Classes and Structures总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j