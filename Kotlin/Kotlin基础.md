# 1 属性与字段

var： 声明为可变的
val： 声明为只读的

```kotlin
var name: String = "Holmes, Sherlock"
var state: String? = null
```

# 2 接口

使用关键字 interface 来定义接口

```kotlin
interface A {
    // 有实现
    fun foo() { print("A") }
    // 无实现
    fun bar() 
}

interface B {
    val name: String
    fun foo() { print("B") }
    fun bar() { print("bar") }
}

class C : A {
    override fun bar() { print("bar") }
}

class D : A, B {
    val firstName: String = ""
    override val name: String get() = "$firstName $lastName"

    override fun foo() {
        super<A>.foo()
        super<B>.foo()
    }

    override fun bar() {
        super<B>.bar()
    }
}
```

# 3 类

Kotlin 中使用关键字 class 声明类

```kotlin
class Person(val name: String) {
    // 类中声明的属性
    var children: MutableList<Person> = mutableListOf<Person>();
    // 构造函数
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
}
```

## 3.1 数据类 

我们经常创建一些只保存数据的类。 在这些类中，一些标准函数往往是从数据机械推导而来的。在 Kotlin 中，这叫做 数据类 并标记为 data。

```kotlin
data class User(val name: String) {
    val age: Int 
}
```

类复制

```kotlin
fun copy(name: String = this.name, age: Int = this.age) = User(name, age)
// 调用
val jack = User(name = "Jack", age = 1)
val olderJack = jack.copy(age = 2)
```

## 3.2 嵌套类

类可以嵌套在其他类中

```kotlin
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2
    }
}

val demo = Outer.Nested().foo() // == 2
```

## 3.3 内部类

标记为 inner 的嵌套类能够访问其外部类的成员。内部类会带有一个对外部类的对象的引用：

```kotlin
class Outer {
    private val bar: Int = 1
    inner class Inner {
        fun foo() = bar
    }
}

val demo = Outer().Inner().foo() // == 1
```

## 3.4 枚举类

``` kotlin
enum class Direction {
    NORTH, SOUTH, WEST, EAST
}
```

枚举类是个特殊的安全类，内部也可以声明相应的方法和属性, 还可以实现接口

```kotlin
enum class IntArithmetics : BinaryOperator<Int>, IntBinaryOperator {
    PLUS {
        override fun apply(t: Int, u: Int): Int = t + u
    },
    TIMES {
        override fun apply(t: Int, u: Int): Int = t * u
    };

    override fun applyAsInt(t: Int, u: Int) = apply(t, u)
}
```

## 3.5 对象

对象使用 objcect。在特殊的情况无需声明类，可以直接创建一个对象。

```kotlin
fun foo() {
    val adHoc = object {
        var x: Int = 0
        var y: Int = 0
    }
    print(adHoc.x + adHoc.y)
}
```

通过 objcect 还可以快速构建单例对象

```kotlin
object DataProviderManager {
    fun registerDataProvider(provider: DataProvider) {
        // ……
    }

}

DataProviderManager.registerDataProvider(……)
```

通过 objcect 还可以创建伴生对象

```kotlin
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}

val instance = MyClass.create()
```


# 4 泛型

与 Swift 类似，Kotlin 中的类也可以有类型参数：

```kotlin
class Box<T>(t: T) {
    var value = t
}

val box: Box<Int> = Box<Int>(1)
// 可推断时，简写为
val box = Box(1) 
```

# 5 集合

Kotlin 标准库提供了一整套用于管理集合的工具

1. List 是一个有序集合，可通过索引（反映元素位置的整数）访问元素
2. Set 是唯一元素的集合
3. Map（或者字典）是一组键值对

并提供了基本集合类型的实现： set、list 以及 map。 一对接口代表每种集合类型：

一个 只读 接口，提供访问集合元素的操作。
一个 可变 接口，通过写操作扩展相应的只读接口：添加、删除和更新其元素。

![](https://www.kotlincn.net/assets/images/reference/collections-overview/collections-diagram.png
)

不可变list

```kotlin
val numbers = listOf("one", "two", "three", "four")
println("Number of elements: ${numbers.size}")
println("Third element: ${numbers.get(2)}")
println("Fourth element: ${numbers[3]}")
println("Index of element \"two\" ${numbers.indexOf("two")}")
```

可变list

``` kotlin
val numbers = mutableListOf(1, 2, 3, 4)
numbers.add(5)
numbers.removeAt(1)
numbers[0] = 0
println(numbers)
```

# 5 高阶函数(block、闭包)

高阶函数是将函数用作参数或返回值的函数。

```kotlin
fun <T, R> Collection<T>.fold(
    initial: R, 
    combine: (acc: R, nextElement: T) -> R
): R {
    var accumulator: R = initial
    for (element: T in this) {
        accumulator = combine(accumulator, element)
    }
    return accumulator
}
```

在上述代码中，参数 combine 具有函数类型 (R, T) -> R，因此 fold 接受一个函数作为参数， 该函数接受类型分别为 R 与 T 的两个参数并返回一个 R 类型的值。

```kotlin
val items = listOf(1, 2, 3, 4, 5)

// Lambdas 表达式是花括号括起来的代码块。
items.fold(0, { 
    // 如果一个 lambda 表达式有参数，前面是参数，后跟“->”
    acc: Int, i: Int -> 
        print("acc = $acc, i = $i, ") 
    val result = acc + i
    println("result = $result")
    // lambda 表达式中的最后一个表达式是返回值：
    result
})

// lambda 表达式的参数类型是可选的，如果能够推断出来的话：
val joinedToString = items.fold("Elements:", { acc, i -> acc + " " + i })

// 函数引用也可以用于高阶函数调用：
val product = items.fold(1, Int::times)
```


# 6 协程

launch 协程执行、runBlocking 阻塞执行

```kotlin
fun main() = runBlocking<Unit> { // 开始执行主协程
    val job = GlobalScope.launch { // 在后台启动一个新的协程并继续
        delay(1000L)
        println("World!")
    }
    println("Hello,") // 主协程在这里会立即执行
    delay(2000L)      // 延迟 2 秒来保证 JVM 存活
    job.join() // 等待直到子协程执行结束
}
```


