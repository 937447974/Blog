## assign 

简单赋值，不更改索引计数。

对基础数据类型 （例如NSInteger，CGFloat）和C数据类型（int, float, double, char, 等），适用简单数据类型。此标记说明设置器直接进行赋值，这也是基础数据类型的默认值。

> 在使用垃圾收集的应用程序中，如果你要一个属性使用assign，且这个类符合NSCopying协议，你就要明确指出这个标记，而不是简单地使用默认值，否则的话，你将得到一个编译警告。这再次向编译器说明你确实需要赋值，即使它是可拷贝的。

## retain

对其他NSObject和其子类进行release旧值，再retain新值。释放旧的对象，将旧对象的指针赋予输入对象，再提高输入对象的索引计数为1

> 指定retain会在赋值时唤醒传入值的retain消息。此属性只能用于Objective-C对象类型，而不能用于Core Foundation对象。(原因很明显，retain会增加对象的引用计数，而基本数据类型或者Core Foundation对象都没有引用计数——译者注)。
> 注意: 把对象添加到数组中时，引用计数将增加对象的引用次数+1。

## copy

建立一个索引计数为1的对象，然后释放旧对象。

> 对NSString 它指出，在赋值时使用传入值的一份拷贝。拷贝工作由copy方法执行，此属性只对那些实行了NSCopying协议的对象类型有效。更深入的讨论，请参考“复制”部分。

## nonatomic 

禁止多线程，变量保护，提高性能。指出访问器不是原子操作，而默认地，访问器是原子操作。

> 在多线程环境下，解析的访问器提供一个对属性的安全访问，从获取器得到的返回值或者通过设置器设置的值可以一次完成，即便是别的线程也正在对其进行访问。
> 如果你不指定nonatomic，在自己管理内存的环境中，解析的访问器保留并自动释放返回的值，如果指定了nonatomic，那么访问器只是简单地返回这个值。

## readonly与readwrite

readonly说明属性是只读的，默认的标记是readwrite。

readwrite说明属性会被当成读写的，这也是默认属性。

## weak和strong

weak弱引用。ARC机制下的assign。

strong强引用。ARC机制下的retain。
 
> 由于arc机制的引入，开发过程中已不在使用assign和retain。

## setter和getter

setter表示当为该属性赋值时，需要使用指定的方法名。
getter表示提前该属性的值时，需要使用指定的方法名。

> 由于.方法的引入，开发过程中已不在使用setter和getter。

## nullable和nonnull

nullable表示对象可以是NULL或nil。
nonnull表示对象不应该为空

> 在Swift与Objective-C混编时，Swift编译器并不知道一个Objective-C对象到底是optional还是non-optional，因此这种情况下编译器会隐式地将Objective-C的对象当成是non-optional。
> 为了解决这一问题，苹果在IOS9中引入了新特性Objective-C的新特性： nullable和nonnull。

----------

本人常用的属性设置如下所示：

``` objc
@property (nonatomic, copy) NSString *name;
@property (nonatomic, strong) NSObject *name;
@property (nonatomic) NSInteger name;
```

基本数据类型的默认值是assign；

对象数据类型的默认值是retain。

&#160;

----------

#Appendix

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2015-10-08 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974