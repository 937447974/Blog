
#1 概述

在c和c的派生语言中，如Object-c和C++，块对象为你创建一个Ad Hoc功能体。在其他编程语言中，一个块对象有时候会称为“关闭（closure）”。在IOS中，我们称呼它为“块（blocks）”。

##1.1 Block的功能

Block是一个匿名内敛的代码集合：

- 就像一个函数具有不同类型的参数列表； 
- 其返回类型可以是void或者声明的类型；
- 从它定义的词法范围可以捕捉其状态；
- 在词法范围内，可以随意修改其状态；
- 在同一词法范围内的其他块中，可以修改它。

你可以传递它到其他线程中延迟执行，或者在当前线程中执行。在block的生命周期，编译器和运行系统将它引用的所有变量都保存为其副本。虽然block可以在C或C++中使用，但更多的时候，它是作为Object-C对象使用。

##1.2 Block的使用

Bolck作为封装的工作单元特别有用，它是小且独立的代码块。它可以同时执行，或者在一个操作完成后作为回调使用。

Block可以作为传统回调函数的替代物，有两个主要的原因：

1. 在方法体的实现过程中，它都可以被调用。因此，在IOS系统框架中，我们经常看见它作为方法的参数。如UIView的动画功能或GCD。

2. Block允许访问局部变量。当你执行一个函数的回调时，你不需要使用一个数据结构，你只需访问局部变量。

#2 声明和创建Block
##2.1 声明Block引用

声明Block类似于声明一个指向函数的指针，只是使用^替代了*。下面是一些有效的Block声明：

```objc
void (^ blockReturningVoidWithVoidArgument)(void);
int (^ blockReturningIntWithIntAndCharArguments)(int, char);
void (^ arrayOfTenBlocksReturningVoidWithIntArgument[10])(int);
```

Block 也支持 (...) 的参数列表。当其参数列表中没有任何类型的参数时，必须使用void作为参数。

编译器通过Block提供的一组完成数据来验证使用它，在参数的传递和返回值的赋值过程中，它是线程安全的。你可以使用任何类型的指针作为Block的引用，反过来也是一样的。但是你不能用*作为块引用，在编译过程中，你也不能计算Block的大小。

当你使用同一个Block在多个地方时，最好的做法是创建一个Block的类型，如：

```objc
typedef float (^ MyBlockType)(float, float);

MyBlockType myFirstBlock = // ... ;
MyBlockType mySecondBlock = // ... ;
```

##2.2 创建Block

使用^来标志要创建一个Block，紧随其后使用一个类型名，并放入（）中，Block的主要操作过程在{}中。下面是一个简单的Block块，我们将其命名为oneForm，其传入和回传的参数类型都是float。

``` objc
float (^oneFrom)(float);
oneFrom = ^(float aFloat) {
	float result = aFloat - 1.0;
	return result;
};
```
如果没有显示的声明Block的返回类型，它也可以通过块内容自动判断返回类型。如果推断参数列表和返回类型都是void，你可以使用void作为参数列表。如果在块中会返回多个值时，你需要保证返回值的类型是一致的。

##3.2 全局Block

在文档的开头，你可以声明一个全局的Block

```objc
#import <stdio.h>

int GlobalInt = 0;
int (^getGlobalInt)(void) = ^{ return GlobalInt; };
```

#3 Block和变量

本节介绍了Block和变量之间的关系，包括内存管理。

##3.1 变量类型

从Block的结构体中，可以有5种不同的变量类型。

从一个函数中，你可以参考三种不同的标准来区分它们。

1. 全局变量包括静态变量。
2. 全局函数
3. 局部变量和从一个封闭的环境中获得的变量。
 
Block也支持其它两个类型的变量：

1. 在函数的级别是__block变量，这些变量是可变的。
2. const(常量)

在Block中使用变量有以下说明：

1. 在Block内可以访问全局变量或静态变量；
2. 就像传参数给函数体，给Block也可以传递参数；
3. 在BLock可以像使用常量一样使用栈(非静态)变量。
4. 通过 __block声明的局部变量在Block内是可变的；
5. 在Block内声明的变量，可以像在函数体内声明的变量一样使用。

下面的例子介绍了怎么使用本地（非静态）变量：

```objc
int x = 123;
void (^printXAndY)(int) = ^(int y) {
	printf("%d %d\n", x, y);
};
printXAndY(456); // prints: 123 456
```

但是，在Block内使用Block外声明的局部变量是错误的：

```objc
int x = 123;
void (^printXAndY)(int) = ^(int y) {
	x = x + y; // error
	printf("%d %d\n", x, y);
};
```

当你想要在Block内改变Blcok外声明的局部变量，需要使用__Block做类型修饰符。

##3.2 __block存储类型

当你使用__block作为存储类型的修饰符后，你就可以在Block内修改它的值。

通过__block声明保存的数据，在所有Block是共享的。也就是说在给定的范围内，多个Block可以共享一个变量。

但是使用__block有两个限制：不可以是可变的数组；其结构不可以包含C99可变类型的数组。

下面的例子介绍了怎么使用__block变量

```objc
__block int x = 123; // x lives in block storage

void (^printXAndY)(int) = ^(int y) {
	x = x + y;
	printf("%d %d\n", x, y);
};
printXAndY(456); // prints: 579 456
// x is now 579
```

下面的例子介绍了，在Block中，不同类型的数据是怎么相互协同工作的。

```objc
extern NSInteger CounterGlobal;
static NSInteger CounterStatic;

{
	NSInteger localCounter = 42;
	__block char localCharacter;

	void (^aBlock)(void) = ^(void) {
		++CounterGlobal;
		++CounterStatic;
		CounterGlobal = localCounter; // localCounter fixed at block creation
		localCharacter = 'a'; // sets localCharacter in enclosing scope
	};

	++localCounter; // unseen by the block
	localCharacter = 'b';

	aBlock(); // execute the block
	// localCharacter now 'a'
}
```

#4 怎么使用Block
##4.1 调用Block

如果你将Block声明为一个变量，你可以如下所示像使用函数一样使用它。

``` objc
int (^oneFrom)(int) = ^(int anInt) {
	return anInt - 1;
};

printf("1 from 10 is %d", oneFrom(10)); // Prints "1 from 10 is 9"

float (^distanceTraveled)(float, float, float) = ^(float startingSpeed, float acceleration, float time) {
	float distance = (startingSpeed * time) + (0.5 * acceleration * time * time);
	return distance;
};

float howFar = distanceTraveled(0.0, 9.8, 1.0); // howFar = 4.9
```

当你将Block作为一个参数传递给一个函数或者方法体时，通常可以创建一个“内联”块。

##4.2 将Blcok作为函数参数

如同其他参数，你也可以将Block作为一个函数的参数。但是，多数情况下，你不需要声明它，你只需要编写它的方法体来实现。下面的例子使用了qsort_b函数，qsort_b函数类似标准的qsort_r函数，在这里我们使用Blcok作为最后一个参数。

```objc
char *myCharacters[3] = { "TomJohn", "George", "Charles Condomine" };

qsort_b(myCharacters, 3, sizeof(char *), ^(const void *l, const void *r) {
	char *left = *(char **)l;
	char *right = *(char **)r;
	return strncmp(left, right, 1);
}); // Block implementation ends at "}"

// myCharacters is now { "Charles Condomine", "George", "TomJohn" }
```

下面的例子介绍如何通过Blcok使用dispatch_apply函数

```objc
#include <dispatch/dispatch.h>
size_t count = 10;
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

dispatch_apply(count, queue, ^(size_t i) {
	printf("%u\n", i);
});
```

##4.3 将Block作为方法体的参数

Cocoa提供了大量的支持Blcok作为参数的方法体，你可以像使用其他参数一样使用它。下面的例子介绍了怎么使用Block作为参数给数组过滤。

``` objc
NSArray *array = @[@"A", @"B", @"C", @"A", @"B", @"Z", @"G", @"are", @"Q"];
NSSet *filterSet = [NSSet setWithObjects: @"A", @"Z", @"Q", nil];

BOOL (^test)(id obj, NSUInteger idx, BOOL *stop);
test = ^(id obj, NSUInteger idx, BOOL *stop) {
	if (idx < 5) {
		if ([filterSet containsObject: obj]) {
			return YES;
		}
	}
	return NO;
};
NSIndexSet *indexes = [array indexesOfObjectsPassingTest:test];
NSLog(@"indexes: %@", indexes);

/*
Output:indexes: <NSIndexSet: 0x10236f0>[number of indexes: 2 (in 2 ranges), indexes: (0 3)]
*/
```

下面的例子介绍了怎么在NSSet数组中通过Block对数据发出索引：

```objc
NSSet *aSet = [NSSet setWithObjects: @"Alpha", @"Beta", @"Gamma", @"X", nil];
NSString *string = @"gamma";
[aSet enumerateObjectsUsingBlock:^(id obj, BOOL *stop) {
	if ([obj localizedCaseInsensitiveCompare:string] == NSOrderedSame) {
	*stop = YES;
	found = YES;
	}
}];
// At this point, found == YES
```

##4.4 需要避免的模式

当你使用^{ ... }使用Block时，下面的例子是需要避免的模式

``` objc
void dontDoThis() {
	void (^blockArray[3])(void); // an array of 3 block references
	for (int i = 0; i < 3; ++i) {
		blockArray[i] = ^{ printf("hello, %d\n", i); };
		// WRONG: The block literal scope is the "for" loop.
	}
}

void dontDoThisEither() {
	void (^block)(void);
	int i = random():
	if (i > 1000) {
		block = ^{ printf("got i at: %d\n", i); };
		// WRONG: The block literal scope is the "then" clause.
}
```

在block使用self的时候会引起循环引用，也应避免。在block中使用self的正确方式如下,使用`__weak `先引用当前类，然后再使用。

```objc
__weak UIViewController *vc = self;
void (^ test)() = ^() {
    // 使用vc中的方法和属性
    vc.title = @"阳君";// 设置标题
};
test();
```

&#160;

----------

#其他

##源代码

https://github.com/937447974/Algorithms

##参考资料

[Blocks Programming Topics](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40007502)

[objc 中的 block](http://blog.ibireme.com/2013/11/27/objc-block/)

[How Do I Declare A Block in Objective-C?](http://fuckingblocksyntax.com)

[梦维block](http://www.dreamingwish.com/articlelist/tag/block)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-09-27 | 博文完成 |
| 2015-11-20 | 添加避免循环引用 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog