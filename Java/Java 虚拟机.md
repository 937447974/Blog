Java 的标准运行环境（JRE）包含 Java API 类库和 Java虚拟机（JVM）两部分。JVM 主要是将字节码文件（.class）解释成为特定的机器码进行运行并对其运行时内存进行管理， 使得 Java 程序具备了 “write once,run anywhere” 的跨平台特性。

# 1 Run-Time Data Areas（运行时数据区）

JVM 在执行程序时会把它管理的内存划分为各种运行时数据区域。其中有些数据区域是在 JVM 启动时创建的，只有在 JVM 退出时才会被销毁。其他数据区域跟随用户线程的创建和结束而建立和销毁。

JVM 所管理的内存包含如下几个运行时数据区域。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017120601.png)

## 1.1 The pc Register（程序计数器）

JVM 可以同时执行多个线程，每个线程都有自己的 pc（程序计数器）寄存器。在任何时候，每个 JVM 线程都只会执行一个方法的代码，该方法称为线程的当前方法。 JVM 字节码解释器通过改变这个计数器的值来选取线程的下一条执行指令，如果这个方法是 Java 方法，那 PC 寄存器就保存 JVM 正在执行的字节码指令地址；如果该方法是 native 的，那 PC 寄存器的值是空（undefined）。

PC 寄存器占用较小的内存空间，容量至少应当能保存一个 returnAddress 类型的数据或一个本地指针，且该区域是唯一不会抛出 OutOfMemoryError 情况的区域。

## 1.2 Java Virtual Machine Stacks（虚拟机栈）

每个 JVM 线程都有一个私有的 JAVA 虚拟机栈，它与线程同时创建，主要保存 Java 方法的栈帧（Stack Frame）。每个方法的执行都会创建一个栈帧，随着方法调用而创建（入栈），随着方法结束而销毁（出栈）。

### 1.2.1 栈帧

栈帧是方法运行的基础结构，其中涉及如下信息：

1. Local Variables（局部变量）：局部变量表存放了编译期可知的各种基本数据类型（boolean, byte, char, short, int, float）、引用对象类型（reference）和 returnAddress 类型。
2. Operand Stacks（操作数栈）：JVM 的指令集基于栈，所有的计算逻辑都需要通过栈来完成，这个栈就是操作数栈。操作数栈的大小由 Java 编译器在编译期计算，计算结果记录在class文件的stack值。
3. Dynamic Linking（动态链接）：每一个栈帧内部都包含一个指向运行时常量池的引用，来支持当前方法的执行过程中实现动态链接。在 Class 文件里面，描述一个方法调用了其他方法，或者访问其成员变量是通过符号引用来表示的。动态链接将这些符号引用方法转换为具体的方法引用，将变量引用转换为运行时的栈偏移量地址。通过这种方式保证我们只需要编译修改的文件即可使程序正常运行。
4. Normal Method Invocation Completion（正常返回）：程序运行正常的返回处理。
5. Abrupt Method Invocation Completion（异常返回）：程序运行异常的返回处理逻辑。

### 1.2.2 异常

JVM 堆栈的错误操作会引起下面的异常：

1. 如果线程请求分配的栈深度超过 JVM 栈允许的最大深度时，会抛出 StackOverflowError 异常。如嵌套方法死循环调用。
2. 如果 JVM 栈动态扩展无法申请到足够的内存，或建立新的线程没有足够的内存去创建对应的虚拟机栈时，会抛出 OutOfMemoryError 异常；

## 1.3 Native Method Stacks（本地方法栈）

本地方法栈和虚拟机栈的功能一样，主要用于保存 native 方法的栈帧。

## 1.4 Heap (堆)

JVM 内控制一个堆，被所有 Java 虚拟机线程共享。堆随 JVM 的创建和结束而建立和销毁，主要用于存储所有类实例和数组。

### 1.4.1 垃圾回收

对象的堆存储是由垃圾回收机制自动管理的，用户无须显示的释放。JVM 使用 G1 收集器（支持并发收集）将堆分为年轻代（Young Generation）和年老代（Old Generation），年轻代分为一个大 Eden Space 和两个小的 Survivor Space，Eden 和 Survivor 默认比例是 8:1。

通过 new 指令的对象绝大多数情况会在 eden 区创建和销毁，需要大内存的数据会直接在 old 区创建。回收时先将 eden 区存活对象复制到一个 survivor0 区，然后清空 eden 区，当这个 survivor0 区也存放满了时，则将 eden 区和 survivo0 区存活对象复制到另一个 survivor1 区，然后清空 eden 和这个 survivor0 区，此时 survivor0 区是空的，然后将 survivor0 区和 survivor1 区交换，即保持 survivor1 区为空， 如此往复。

每一次的gc，存活的对象年龄 +1，默认对象经过了 15 次 GC 还是存活的时候，会移入到 old 区。当

整个对象的存活顺序如下所示经过 young(eden -> survivor) -> old。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017120602.png)

### 1.4.2 异常

如果 Heap 满了，且垃圾回收也无法回收内存，又无法申请更多的内存时，会抛出 OutOfMemoryError 异常。

很多时候我们希望更快的回收内存，如对象未进入年老代就被清理了，但是对象线程逃逸，且线程又没执行完毕，很可能对象就进入了年老代，长时间停留在内存中。Java 为解除这种强引用对象问题，对此提供了软引用、弱引用和虚引用对象。

1. 软引用（SoftReference）：软引用是用来描述一些有用但并不是必需的对象，只有在内存不足的时候JVM才会回收该对象。
2. 弱引用（WeakReference）：弱引用也是用来描述非必需对象的，当JVM进行垃圾回收时，无论内存是否充足，都会回收被弱引用关联的对象。
3. 虚引用（PhantomReference）：虚引用和前面的软引用、弱引用不同，它并不影响对象的生命周期。如果一个对象与虚引用关联，则跟没有引用与之关联一样，在任何时候都可能被垃圾回收器回收。

## 1.5 Method Area（方法区）

方法区类似于一个传统语言的编译代码的存储区域，被所有 Java 虚拟机线程共享。它存储每个类的结构，比如运行时常量池、字段和方法数据、以及方法和构造函数的代码，还包括一些在类、实例、接口初始化时用到的特殊方法。方法区又名非堆，和 java 堆区分，代表它是提供给 JVM 使用的内存区域。

方法区不是垃圾回收的主要工作区域，当它也有垃圾回收机制，如卸载一个不使用的类。

### 1.5.1 Run-Time Constant Pool（运行时常量池）

运行常量池是方法区的一部分，包含了若干种不同的常量。

1. class 文件结构中的常量池。
2. 运行期栈动态链接才知道的方法或字段的直接引用。
3. 运行时可能创建的新常量，如 String 类 intern() 方法。

### 1.5.2 异常

理论上方法区是不会抛异常，不过当方法区需要内存扩展且无法申请时，会抛 OutOfMemoryError 异常。如堆将内存快耗尽了，此时方法区加载未使用的类将无法申请到内存，抛异常。

# 2 Class 字节码

测试代码

```java
public class Test {
    private int a = 3;
    private static Integer b = 5;
    public String c = "YJ";

    public static void main(String[] args) throws Exception {
        Test test = new Test();
        test.a = 8;
        b = 8;
    }

    private String test1() {
        return "YJJ";
    }
    
}
```

编译上面的 Test.java 后，执行 `javap -v Test` 命令打开 Test.class。得到如下所示的数据

```java
Classfile /Users/didi/Desktop/GitHub/Java/java/target/test-classes/Test.class
  Last modified 2017年12月6日; size 823 bytes
  MD5 checksum 17782135ec29c6b388028d5257adf83c
  Compiled from "Test.java"
public class Test
  minor version: 0
  major version: 52
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #5                          // Test
  super_class: #10                        // java/lang/Object
  interfaces: 0, fields: 3, methods: 4, attributes: 1
Constant pool:
   #1 = Methodref          #10.#36        // java/lang/Object."<init>":()V
   #2 = Fieldref           #5.#37         // Test.a:I
   #3 = String             #38            // YJ
   #4 = Fieldref           #5.#39         // Test.c:Ljava/lang/String;
   #5 = Class              #40            // Test
   #6 = Methodref          #5.#36         // Test."<init>":()V
   #7 = Methodref          #41.#42        // java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
   #8 = Fieldref           #5.#43         // Test.b:Ljava/lang/Integer;
   #9 = String             #44            // YJJ
  #10 = Class              #45            // java/lang/Object
  #11 = Utf8               a
  #12 = Utf8               I
  #13 = Utf8               b
  #14 = Utf8               Ljava/lang/Integer;
  #15 = Utf8               c
  #16 = Utf8               Ljava/lang/String;
  #17 = Utf8               <init>
  #18 = Utf8               ()V
  #19 = Utf8               Code
  #20 = Utf8               LineNumberTable
  #21 = Utf8               LocalVariableTable
  #22 = Utf8               this
  #23 = Utf8               LTest;
  #24 = Utf8               main
  #25 = Utf8               ([Ljava/lang/String;)V
  #26 = Utf8               args
  #27 = Utf8               [Ljava/lang/String;
  #28 = Utf8               test
  #29 = Utf8               Exceptions
  #30 = Class              #46            // java/lang/Exception
  #31 = Utf8               test1
  #32 = Utf8               ()Ljava/lang/String;
  #33 = Utf8               <clinit>
  #34 = Utf8               SourceFile
  #35 = Utf8               Test.java
  #36 = NameAndType        #17:#18        // "<init>":()V
  #37 = NameAndType        #11:#12        // a:I
  #38 = Utf8               YJ
  #39 = NameAndType        #15:#16        // c:Ljava/lang/String;
  #40 = Utf8               Test
  #41 = Class              #47            // java/lang/Integer
  #42 = NameAndType        #48:#49        // valueOf:(I)Ljava/lang/Integer;
  #43 = NameAndType        #13:#14        // b:Ljava/lang/Integer;
  #44 = Utf8               YJJ
  #45 = Utf8               java/lang/Object
  #46 = Utf8               java/lang/Exception
  #47 = Utf8               java/lang/Integer
  #48 = Utf8               valueOf
  #49 = Utf8               (I)Ljava/lang/Integer;
{
  public java.lang.String c;
    descriptor: Ljava/lang/String;
    flags: (0x0001) ACC_PUBLIC

  public Test();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: aload_0
         5: iconst_3
         6: putfield      #2                  // Field a:I
         9: aload_0
        10: ldc           #3                  // String YJ
        12: putfield      #4                  // Field c:Ljava/lang/String;
        15: return
      LineNumberTable:
        line 12: 0
        line 13: 4
        line 15: 9
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      16     0  this   LTest;

  public static void main(java.lang.String[]) throws java.lang.Exception;
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=2, args_size=1
         0: new           #5                  // class Test
         3: dup
         4: invokespecial #6                  // Method "<init>":()V
         7: astore_1
         8: aload_1
         9: bipush        8
        11: putfield      #2                  // Field a:I
        14: bipush        8
        16: invokestatic  #7                  // Method java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
        19: putstatic     #8                  // Field b:Ljava/lang/Integer;
        22: return
      LineNumberTable:
        line 18: 0
        line 19: 8
        line 20: 14
        line 21: 22
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      23     0  args   [Ljava/lang/String;
            8      15     1  test   LTest;
    Exceptions:
      throws java.lang.Exception

  static {};
    descriptor: ()V
    flags: (0x0008) ACC_STATIC
    Code:
      stack=1, locals=0, args_size=0
         0: iconst_5
         1: invokestatic  #7                  // Method java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
         4: putstatic     #8                  // Field b:Ljava/lang/Integer;
         7: return
      LineNumberTable:
        line 14: 0
}
SourceFile: "Test.java"
```

JVM 加载上面的字节码文件后会得到一个如下所示的 ClassFile 对象。

```java
ClassFile {
       u4             magic;
       u2             minor_version;
       u2             major_version;
       u2             constant_pool_count;
       cp_info        constant_pool[constant_pool_count-1];
       u2             access_flags;
       u2             this_class;
       u2             super_class;
       u2             interfaces_count;
       u2             interfaces[interfaces_count];
       u2             fields_count;
       field_info     fields[fields_count];
       u2             methods_count;
       method_info    methods[methods_count];
       u2             attributes_count;
       attribute_info attributes[attributes_count];
}
```

其中包含魔术、版本号、常量池、类信息、类的构造函数、类中所包含的方法信息以及类（成员）变量信息。

## 2.1 魔术

魔术用于校验 class 文件是否可被加载，它的值是固定的 0xCAFEBABE。

使用如下命令打开 Test.class 的 16 进制。

```java
vi -b Test.class
:%!xxd
```

执行命令后可以看见文件的前 4 个字节是魔术 0xCAFEBABE。如果开始四个字节不是 0xCAFEBABE，则JVM 将会认为该文件不是 .class 文件，并拒绝解析。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017120603.png)

## 2.2 版本号

版本号分为主版本号（major_version）和次版本号（minor_version），在 16 进制的后 4 字节 0x00000034(先小版本后大版本)，可知版本号为（0，52），即 52.0，目前已发布的 Version 为 1.1(45)~1.9(53)，可知 Test.class 是在 JDK 1.8 编译的，如果 JVM 发现 class 版本号大于JVM 版本号，则会报错 “java.lang.UnsupportedClassVersionError: ... : Unsupported major.minor version 52.0”

## 2.3 常量池

常量池是 .class 字节码文件中非常重要的核心内容，一个 Java 类中绝大多数的信息都由常量池描述，尤其是 Java 中定义的变量和方法，都由常量池保存。JVM 方法区中的运行时常量池就是用来保存每个 Java 类所对应的常量池的信息的。

## 2.4 访问标识

常量池数组之后跟着的是 access_flags 结构，代表访问标志符号，该标志用于标志类或接口层次的访问信息。

access_flags 的可选值如下所示

| Flag Name | Value | Interpretation |
| --- | --- | --- || ACC_PUBLIC | 0x0001 | 是否为 public 类型 || ACC_FINAL | 0x0010 | 是否声明为 final || ACC_SUPER | 0x0020 | 是否允许使用 invokespecial 字节码指令 || ACC_INTERFACE | 0x0200 | 声明为接口 || ACC_ABSTRACT | 0x0400 | 声明 abstract 类型，不能被实例化。|| ACC_SYNTHETIC | 0x1000 | 声明这个类并非由用户代码产生 || ACC_ANNOTATION | 0x2000 | 声明为注解 || ACC_ENUM | 0x4000 | 声明为枚举 || ACC_MODULE | 0x8000 | 声明为模块 |

Test.class 的访问标识 `flags: (0x0021) ACC_PUBLIC, ACC_SUPER`，对应源代码 `public class Test`。

## 2.5 当前类

当前类 this_class 记录当前类的权限定名（包名+类名），其值指向常量池中对象的索引值。

如 Test.class 字节码解析类名

```
this_class: #5
#5 = Class  #40            // Test
#40 = Utf8  Test
```

## 2.6 父类

父类 super_class 记录当前类的父类全限定名，其值指向常量池中对应的索引值。

如 Test.class 字节码解析父类名

```java
super_class: #10                        // java/lang/Object
#10 = Class       #45            // java/lang/Object
#45 = Utf8        java/lang/Object
```

## 2.7 接口

interfaces 记录了当前类实现的接口信息。

## 2.8 字段

fields 记录当前类成员变量和类变量（静态变量）。fields 内部由 fields_info 保存变量的详细信息。

```
field_info {
       u2             access_flags; // 访问标识
       u2             name_index; // 名称引用
       u2             descriptor_index; // 变量的类型信息引用
       u2             attributes_count; // 字段内的属性个数 
       attribute_info attributes[attributes_count]; // 字段内的属性信息
}
```

## 2.9 方法

methods 记录当前类中包含的方法。Test.class 中的方法个数是4，实际源码中的方法个数是2，这是因为编译器编译的时候会默认添加 `static{}` 静态初始化（类中有静态变量）和类实例化的初始化方法。 methods 内由 methods_info 保存变量的详细详细。

```
method_info {
       u2             access_flags; // 访问标识
       u2             name_index; // 方法名
       u2             descriptor_index; // 方法的类型信息引用
       u2             attributes_count; // 参数属性个数
       attribute_info attributes[attributes_count]; // 参数属性内容
}
```

## 2.10 属性

attribute 记录当前类的所有属性。对于 class 来说，必须有一个模块属性。 attributes 内由 attribute_info 保存属性的详细详细。

```
attribute_info {
       u2 attribute_name_index; // 属性名引用
       u4 attribute_length; // 属性长度
       u1 info[attribute_length]; // 属性内信息
}
```

# 3 类的生命周期

一个 Java 字节码文件从加载到卸载的整个生命过程，总共要经历 5 个阶段：加载、链接、初始化、使用和卸载。其中链接又分支验证、准备和解析。前面的常量池解析、类字段解析和方法解析都属于加载阶段的一部分。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017121001.png)

## 3.1 加载

Java 程序的所有数据结构和算法都封装在类型之中，这也是面向对象编程语言的一大特色。当 JVM 执行一个 Java 类所封装的算法之前，首先要做的一件事便是字节码文件解析，字节码文件解析包含3个主要的过程常量池解析、类字段解析和方法解析。通过类字段解析，JVM 能够分析 Java 类的数据结构；通过方法解析，JVM 能够分析出 Java 类所封装的算法逻辑。而无论是数据结构还是方法信息，很多相关的信息都封装与常量池中，所以 JVM 会先解析常量池，后解析字段和方法信息。

Java 字节码的加载主要是通过 ClassLoader(类加载器) 完成的，ClassLoader 分为 bootstrap class loader（引导类加载器）和用户自定义的类加载器。引导类加载器基本上可以加载我们使用的所有类。自定义加载器使得我们可以随意加载各种源的字节码问题，如网络下载的文件即可通过自定义类加载器加载。 

Java 的类加载过程实际是将字节码格式的 Java 类转换成机器能够识别的内存类模板快照。

## 3.2 链接

Java 运行过程中实际是这些的 class 类，前面 JVM 加载之后会生成内存类的模板，但它还不可以直接使用，还需要链接，如将符号化的引用转换为直接引用。

在链接过程中需要创建新的数据结构，这个过程如果内存不足，会抛出 OutOfMemoryError 错误。

经过链接操作，JVM 还是无法生成一个对应 class 类，会抛出 ClassNotFoundException 错误。多数情况下编译时，编译器会自动检测这个错误。但是自定义的类加载器，还是有可能加载失败，导致这个错误的出现。

### 3.2.1 验证

验证在加载过程中也有，如验证字节码文件的数据格式、魔术、版本号等。链接过程的验证，主要是确保类或接口的二进制信息是符合当前虚拟机的规范，如符号引用的验证，确保程序能够正确执行。

### 3.2.2 准备

准备包括为类或接口创建静态字段，并准备它们的默认值。这并不需要执行任何 JVM 代码，静态字段的显式初始化将在初始化阶段执行，而不是准备阶段。

### 3.2.3 解析

解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程。解析动作主要针对类或接口、字段、类方法、接口方法、方法类型、方法句柄和调用点限定符7类符号引用进行。

## 3.3 初始化

类初始化阶段是类加载的最后一步，初始化并非类的初始化，而是调用 Java 类的 `<clinit>()` 方法。该方法是由编译器在编译期间自动生成的，当 Java 类中出现 static 字段或 static {} 块操作时，所编译出来的字节码文件则保护一个 <clinit>() 方法，实质是执行 static() 方法。

```java
public class Test {
    static  {
        name = "Y";
    }
    private static String name = "YJ";
}
```

编译为字节码文件后

```
static {};
	descriptor: ()V
	flags: (0x0008) ACC_STATIC
	Code:
	  stack=1, locals=0, args_size=0
	     0: ldc           #8                  // String Y
	     2: putstatic     #9                  // Field name:Ljava/lang/String;
	     5: ldc           #2                  // String YJ
	     7: putstatic     #9                  // Field name:Ljava/lang/String;
	    10: return
	  LineNumberTable:
	    line 16: 0
	    line 18: 5
```

可知执行初始化方法后，name 最后的值是 YJ。上面是种错误的写法，因为我们需要的是 name 的值为 Y。正确的写法是：

```java
public class Test {
    private static String name = "YJ";
    static  {
        name = "Y";
    }
}
```

## 3.4 使用

我们所编写的代码，主要是使用阶段的代码。

```
import package.Test;
Test test = new Test();
```

对类的使用主要是通过 import 导入类，new 实例化类。import 是别名操作，只在编译期起作用，运行期的时候会转换为 `Test test = new package.Test();` 执行。JVM 碰到 new 指令会判断元空间是否存在 class 类，如果不存在会执行前面的加载、链接和初始化操作。如果存在则直接使用，这就是 JVM 为了性能内存，执行的慢分配过程。

主要分为如下几点

1. 若 Java 类尚未被解析，则直接进入慢分配，不会使用快速分配策略。
2. 快速分配。如果没有开启栈上分配或不符合条件则会进行 TLAB 分配。
3. 快速分配。如果 TLAB 分配不成功，则尝试在 eden 区分配。
4. 如果 Eden 区分配失败，则会进入慢分配流程。
5. 如果对象满足了直接进入老年代的条件，那就直接分配在老年代。
6. 如果开启逃逸分析，JVM 则会执行栈上分配的优化方案。

JVM 启动的时候会把使用频率高的类完成预加载操作。这就是为什么启动一个空的 main(String[] args) 方法后，会通过 jconsole 看见已经加载了2000多个类。

栈上分配的好处不言而喻，无须 gc 操作，但 Java 类型不能太大，包含的字段不能太多，比较栈空间是有栈顶和栈底。

## 3.5 卸载

Java 类的卸载是随 ClassLoader 的回收而卸载的，实际通过静态代码绑定是跟随 JVM 的停止而卸载。

&#160;

----------

# Appendix

## Related Documentation

[揭秘Java虚拟机（JVM设计原理与实现）](http://product.dangdang.com/25111315.html)

[深入Java虚拟机(第二版)](http://product.dangdang.com/23259731.html)

[The Java® Virtual Machine Specification](https://docs.oracle.com/javase/specs/jvms/se9/html/index.html)

[Java内存区域 JVM运行时数据区](http://blog.csdn.net/tjiyu/article/details/53915869)

[Using JConsole](https://docs.oracle.com/javase/9/management/using-jconsole.htm#JSMGM-GUID-77416B38-7F15-4E35-B3D1-34BFD88350B5)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-12-10 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974