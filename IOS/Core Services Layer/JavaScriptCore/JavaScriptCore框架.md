[UIWebView基础(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49619847)

[UIWebView进阶(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49638523)

[JavaScriptCore框架](http://blog.csdn.net/y550918116j/article/details/49666443)

[UIWebView高级(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49684127)

----------

在WWDC 2013上，苹果公司推出了JavaScriptCore框架，如果感兴趣的朋友可以查看[WWDC 2013: Integrating JavaScript into Native Apps](https://developer.apple.com/videos/play/wwdc2013-615/)。这是一个基于JavaScript的框架，它完美的以面向对象的方式实现js和oc的交互。

JavaScriptCore涉及到几个核心类：

- **JSContext**：JavaScript的执行环境；
- **JSValue**：JSValue则可以说是JavaScript和Object-C之间互换的桥梁；
- **JSManagedValue**：JSValue的内存管理器；
- **JSVirtualMachine**：JSVirtualMachine为JavaScript的运行提供了底层资源；
- **JSExport**：协议，oc和js的通信协议。

JavaScriptCore让OC和JS自动完成交互。由于js是单线程的，JavaScriptCore的操作也是线程安全的。

>开发过程中需要引入的库：`#import <JavaScriptCore/JavaScriptCore.h>`

#JSContext

JSContext是JavaScript的执行环境，所有的JavaScript的执行都是基于这个环境。JSContext同时也管理JavaScript对象的生命周期，每一个JSValue都和JSContext强引用关联。

JSContext通过`- (JSValue *)evaluateScript:(NSString *)script;`执行JavaScript方法，并返回一个JSValue对象。

```
// 初始化
JSContext *context = [[JSContext alloc] init];
context = [JSContext currentContext];
```

#JSValue

JSValue是JavaScript和Object-C之间互换的桥梁，它提供了多种方法可以方便地把JavaScript数据类型转换成Objective-C，或者是转换过去。

下列是Objective-C类型和JavaScript类型的对应关系图。

| Objective-C类型 | JavaScript类型 |
| :----: | :----: |
| nil | undefined |
| NSNull | null |
| NSString | string |
| NSNumber | number, boolean |
| NSDictionary | Object object |
| NSArray | Array object |
| NSDate | Date object |
| NSBlock (1) | Function object (1) |
| id (2) | Wrapper object (2) |
| Class (3) | Constructor object (3) |

通过Objective-C创建JSValue对象，可以使用如下常用方法。

```
+ valueWithBool:(BOOL)value inContext:(JSContext *)context;+ valueWithDouble:(double)value inContext:(JSContext *)context;+ valueWithInt32:(int32_t)value inContext:(JSContext *)context;+ valueWithUInt32:(uint32_t)value inContext:(JSContext *)context;+ valueWithNullInContext:(JSContext *)context;+ valueWithUndefinedInContext:(JSContext *)context;+ valueWithNewObjectInContext:(JSContext *)context;+ valueWithNewArrayInContext:(JSContext *)context;+ valueWithNewRegularExpressionFromPattern:(NSString *)pattern flags:(NSString *)flags inContext:(JSContext *)context;+ valueWithNewErrorFromMessage:(NSString *)message inContext:(JSContext *)context;
```

你还可以使用通配方法创建。

```
+ valueWithObject:(id)value inContext:(JSContext *)context;
```

如果你想访问JSValue的值，可以使用如下常用方法。

```
- (BOOL)toBool;- (double)toDouble;- (int32_t)toInt32;- (uint32_t)toUInt32;- (NSNumber *)toNumber;- (NSString *)toString;- (NSDate *)toDate;- (NSArray *)toArray;- (NSDictionary *)toDictionary;
- (id)toObject;- (id)toObjectOfClass:(Class)expectedClass;
```

#JSExport

JavaScript可以脱离prototype继承完全用JSON来定义对象，但是Objective-C编程里可不能脱离类和继承写代码。所以JavaScriptCore就提供了JSExport作为两种语言的互通协议。JSExport中没有约定任何的方法，连可选的(@optional)都没有，但是所有继承了该协议(@protocol)的协议（注意不是Objective-C的类(@interface)）中定义的方法，都可以在JSContext中被使用。

简单点说就是你定义的类要想在JSContext中被使用，就像下面这样写。

```
@protocol MyClassJavaScriptMethods <JSExport>
- (void)foo;
@end

@interface MyClass : NSObject <MyClassJavaScriptMethods>
- (void)foo;
- (void)bar;
@end
```

#基本类型转换

下面是一些基本类型转换操作，你可以使用XCTestCase做这些测试。

```
#import <XCTest/XCTest.h>
#import <JavaScriptCore/JavaScriptCore.h>

@interface JavaScriptCoreTests : XCTestCase

@property (nonatomic, strong) JSContext *context;///< JavaScript运行环境

@end

@implementation JavaScriptCoreTests

- (void)setUp {
    [super setUp];
    self.context = [[JSContext alloc] init];
    // self.context = [JSContext currentContext];
    
}

- (void)tearDown {
    [super tearDown];
}

- (void)testExample {
    // 计算2+2    JSValue *result = [self.context evaluateScript:@"2 + 2"];    NSLog(@"2 + 2 = %d", [result toInt32]);
    // 数组操作
    [self.context evaluateScript:@"var array = [\"阳君\", 937447974 ];"];
    JSValue *jsArray = self.context[@"array"];// 下标获取值
    // 判断是否为数组
    if (![jsArray isArray]) {
        return;
    }
    NSLog(@"JS Array: %@; Length: %@", jsArray, jsArray[@"length"]);
    jsArray[1] = @"blog"; // 修改
    jsArray[7] = @YES; // 数组越界时自动添加
    NSLog(@"JS Array: %@; Length: %d", jsArray, [jsArray[@"length"] toInt32]);
    // 转换为array
    NSArray *nsArr = [jsArray toArray];
    NSLog(@"NSArray: %@", nsArr);
}

@end

```

> 以下关于JavaScriptCore的测试为节约篇幅只显示核心代码，不在显示整个文件的代码。

#函数

你还可以在JSContext中添加函数，这里我们通过jS实现一个递归算法计算`1*2*3*....*n`。

```
#pragma mark 函数测试
- (void)testFunctions {
    /* js代码
     var factorial = function(n) {        if (n < 0)            return;        if (n === 0)            return 1;        return n * factorial(n - 1);     };
     */
    // 计算1*2*3....*n
    NSMutableString *factorialScript = [NSMutableString stringWithCapacity:10];
    [factorialScript appendString:@"var factorial = function(n) { "];
    [factorialScript appendString:@"if (n < 0) return; "];
    [factorialScript appendString:@"if (n === 0) return 1; "];
    [factorialScript appendString:@"return n * factorial(n - 1); };"];    [_context evaluateScript:factorialScript];    JSValue *function = _context[@"factorial"]; // 提取函数    JSValue *result = [function callWithArguments:@[@5]];    NSLog(@"factorial(5) = %d", [result toInt32]);
}
```

#Blocks

各种数据类型可以转换，Objective-C的Block也可以传入JSContext中当做JavaScript的方法使用。

>如果你不懂什么是Blocks，可以阅读我的博文《[Blocks Programming](http://blog.csdn.net/y550918116j/article/details/48767315)》

这里我们通过Blocks实现如下需求：

1. 创建一个类user，其中包含name和qq属性。
2. 修改属性name的值，即`user.name = newName;` 

```
#pragma mark Blocks测试
- (void)testBlocks {
    
    // 设计一个user类，其中有name和qq属性
    self.context[@"user"] = ^{
        // 如果想使用JSContext，必须使用[JSContext currentContext]，避免循环引用        JSValue *object = [JSValue valueWithNewObjectInContext:[JSContext currentContext]];
        // 属性传入
        [object setValue:@"阳君" forProperty:@"name"];
        // 通过下标传入        object[@"qq"] = @"937447974";        return object;    };
    JSValue *user = [self.context evaluateScript:@"user()"];
    NSLog(@"name:%@, qq:%@", user[@"name"], [user valueForProperty:@"qq"]);
    
    // 修改name值
    self.context[@"setName"] = ^(JSValue *user, JSValue *name) {
        // 也可以这样获得传入参数
        NSArray *args = [JSContext currentArguments];
        NSLog(@"currentArguments:%@", args);
        user[@"name"] = name;
    };
    JSValue *setName = self.context[@"setName"];
    [setName callWithArguments:[NSArray arrayWithObjects:user, @"YangJun",nil]];
    NSLog(@"name:%@", user[@"name"]);
    
}
```

其中有几个值得注意的地方：

1. 在Blocks内，想获得JSContext时，需用`[JSContext currentContext]`获得，详见创建user类的实现；
2. 在Block内不要直接使用其外部定义的JSValue，应该将其当做参数传入到Block中，否则会造成循环引用使得内存无法被正确释放，详见修改name值的实现。

#异常处理

Objective-C和Swift的异常会在运行时被Xcode捕获，而在JSContext中执行的JavaScript如果出现异常，只会被JSContext捕获并存储在exception属性上，而不会向外抛出。

时时刻刻检查JSContext对象的exception是否不为nil显然是不合适，更合理的方式是给JSContext对象设置exceptionHandler。点击查看其需要的blocks格式如下。

```
@property (copy) void(^exceptionHandler)(JSContext *context, JSValue *exception);
```

现在来做一个js出错的测试。

```
#pragma mark 异常捕获
- (void)testError {
    // 通过exceptionHandler捕获js错误
    self.context.exceptionHandler = ^(JSContext *con, JSValue *exception) {
        NSLog(@"%@", exception);
        con.exception = exception;
    };
    [self.context evaluateScript:@"阳君-937447974"];
}
```

#JSExport协议

上面我们使用Blocks实现了类的创建和属性的赋值。下面我们使用JSExport来创建可维护User类，其实现的需求如下。

1. 创建一个类user，其中包含name和qq属性。
2. 修改属性name的值，即`user.name = newName;` 

首先我们根据JSExport协议制作User类，下面是核心代码。

User.h

```
//
//  User.h
//  UIWebView
//
//  Created by yangjun on 15/11/5.
//  Copyright © 2015年 六月. All rights reserved.
//

#import <Foundation/Foundation.h>
#import <JavaScriptCore/JavaScriptCore.h>

/** JavaScript可调的协议*/
@protocol UserProtocol <JSExport>

@property (nonatomic, copy) NSString *name; ///< 姓名
@property (nonatomic, copy) NSString *qq;   ///< qq号码

- (NSString *)description; // 打印信息

@end

/** 用户*/
@interface User : NSObject <UserProtocol>

@end
```

这里定义了UserProtocol协议，其实现JSExport协议。在这个协议中，我们声明了需要暴露给js的属性和方法。

接下来声明了一个User类，然后让它实现UserProtocol即可。由于我们在协议中定义了相关属性和方法，在User类中只需实现方法，对于属性需要使用@synthesize通知编译器实现getter/setter方法。下面是User.m的核心代码

```
//
//  User.m
//  UIWebView
//
//  Created by yangjun on 15/11/5.
//  Copyright © 2015年 六月. All rights reserved.
//

#import "User.h"

@implementation User

// 由于属性在代理里面，需要通知编译器实现getter/setter方法
@synthesize name,qq;

- (NSString *)description {
    return [NSString stringWithFormat:@"name:%@; qq:%@", self.name, self.qq];
}

@end
```

使用XCTestCase测试。

```
- (void)testJSExport {
    User *user = [[User alloc] init];
    self.context[@"user"] = user; // 对象绑定
    // 修改值
    user.name = @"阳君";
    [self.context evaluateScript:@"user.qq = 937447974"];
    // 打印
    NSLog(@"oc:%@", user); //由于继承NSObJect，会自动调用description方法
    JSValue *description = [self.context evaluateScript:@"user.description()"];
    NSLog(@"js:%@", description);
}
```

&#160;

----------

#其他

##参考资料

[WWDC 2013: Integrating JavaScript into Native Apps](https://developer.apple.com/videos/play/wwdc2013-615/)

[iOS与JS交互实战篇（ObjC版）](http://mp.weixin.qq.com/s?__biz=MzIzMzA4NjA5Mw==&mid=214063688&idx=1&sn=903258ec2d3ae431b4d9ee55cb59ed89&scene=18#rd)

[iOS7新JavaScriptCore框架入门介绍](http://www.cnblogs.com/ider/p/introduction-to-ios7-javascriptcore-framework.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-5 | 关于JavaScriptCore的相关介绍 |
| 2015-11-6 | 增加相关文章的链接《[UIWebView基础(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49619847)》、《[UIWebView进阶(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49638523)》、《[JavaScriptCore框架](http://blog.csdn.net/y550918116j/article/details/49666443)》和《[UIWebView高级(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49684127)》。|

&#160;

----------

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog