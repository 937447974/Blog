#1 KVC

KVC（Key-value coding）键值编码。简单来说，是可以通过对象属性名称（Key）直接给属性值（value）编码（coding）“编码”可以理解为“赋值”。其内部运用了 isa-swizzling 技术。isa-swizzling 就是类型混合指针机制。KVC 主要通过 isa-swizzling，来实现其内部查找定位的。

##1.1 使用

```objc
@property (class, readonly) BOOL accessInstanceVariablesDirectly; // 是否禁用KVC
- (nullable id)valueForKey:(NSString *)key;                 // getter
- (void)setValue:(nullable id)value forKey:(NSString *)key; // setter
```

通过 setter 方法我们就可以动态给 readonly 的对象赋值。key 可以是属性也可以是_属性。

##1.2 底层调用

假如我们调用`[[NSObject alloc] setValue:nil forKey:@"property"];`，其KVC调用如下所示：

1. 去模型中查找有没有对应的 setter 方法：例如：setProperty 方法，有就直接调用这个 setter 方法给模型这个属性赋值;
2. 如果找不到 setter 方法，接着就会去寻找有没有 property 属性，如果有，就直接访问模型中的property属性，进行赋值;
3. 如果找不到 property 属性，接着又会去寻找 _property 属性，如果有，直接进行赋值
4. 如果都找不到会报出如下所示的崩溃信息。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017032101.png)

从崩溃信息我们可以发现如下信息

1. KVC 使用了 OSSpinLock 锁
2. 其存储信息可分散在 CFSetCreateMutable -> CFHash -> CFSetGetValue -> CFSetAddValue

#2 KVO

Key-Value Observing (KVO) 建立在 KVC 之上，它通过观察一个对象的 KVC key path 值的变化向外发送通知。

##2.1 使用

```objc
// 1. 注册观察者，实施监听
- (void)addObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath options:(NSKeyValueObservingOptions)options context:(nullable void *)context;

// 2. 在回调方法中处理属性发生的变化
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context {
    if (context == <#context#>) {
        <#code to be executed upon observing keypath#>
    } else {
        [super observeValueForKeyPath:keyPath ofObject:object change:change context:context];
    }
}

// 3. 移除观察者
- (void)removeObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath context:(nullable void *)context NS_AVAILABLE(10_7, 5_0);
- (void)removeObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath;
```

> 当父类和子类同时 KVO 同一个对象时，在 `dealloc` 移除引起崩溃时 应对 context 赋值，移除时也应通过`(void)removeObserver: forKeyPath: context:` 方法移除。

## 

&#160;

----------

#Appendix

##Related Documentation

1. [iOS--KVO的实现原理与具体应用](http://www.cnblogs.com/azuo/p/5442319.html)
2. [【原】iOS下KVO使用过程中的陷阱](http://www.cnblogs.com/wengzilin/p/4346775.html)
3. [KVC 与 KVO 理解](https://magicalboy.com/kvc_and_kvo/)
4. [KVC, KVO实现原理剖析](http://www.jianshu.com/p/37a92141077e)
5. [KVC与Runtime结合使用(案例)及其底层原理](http://www.cnblogs.com/junhuawang/p/5802516.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-03-21 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974