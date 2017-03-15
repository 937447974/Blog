本文档描述了OS X 2.0 objective - c运行时库函数和数据结构的支持。相关实现功能在`objc/objc-runtime.h`共享库中。

> 所有const char *使用UTF-8转码

#1 Functions

所有方法前带OBJC_EXPORT

##1.1 Working with Classes

```objc
// 获取类名
const char *class_getName(Class cls)
// 获取类的父类
Class class_getSuperclass(Class cls)
// 设置类的父类
Class class_setSuperclass(Class cls, Class newSuper)
// 判断给定的Class是否是一个元类
BOOL class_isMetaClass(Class cls)
// 获取初始化class需要的内存大小
size_t class_getInstanceSize(Class cls)
// 获取类中指定名称实例成员变量的信息
Ivar class_getInstanceVariable(Class cls, const char *name)
// 获取类成员变量的信息
Ivar class_getClassVariable(Class cls, const char *name)
// 添加成员变量
BOOL class_addIvar(Class cls, const char *name, size_t size, int8_t alignment, const char *types) 
// 获取整个成员变量列表
Ivar *class_copyIvarList(Class cls, unsigned int *outCount)
// 获取strong成员
const uint8_t *class_getIvarLayout(Class cls)
// 设置strong成员
void class_setIvarLayout(Class cls, const uint8_t *layout)
// 获取weak成员
const uint8_t *class_getWeakIvarLayout(Class cls)
// 设置weak成员
void class_setWeakIvarLayout(Class cls, const char *layout)
// 获取指定的属性
objc_property_t class_getProperty(Class cls, const char *name)
// 获取属性列表
objc_property_t * class_copyPropertyList(Class cls, unsigned int *outCount)
// 添加方法
BOOL class_addMethod(Class cls, SEL name, IMP imp, const char *types)
// 获取实例方法
Method class_getInstanceMethod(Class aClass, SEL aSelector)
// 获取类方法
Method class_getClassMethod(Class aClass, SEL aSelector)
// 获取所有方法的数组
Method * class_copyMethodList(Class cls, unsigned int *outCount)
// 替代方法的实现
IMP class_replaceMethod(Class cls, SEL name, IMP imp, const char *types)
// 返回方法的具体实现
IMP class_getMethodImplementation(Class cls, SEL name)
// 获取类中的方法的实现,该方法的返回值类型为struct
IMP class_getMethodImplementation_stret(Class cls, SEL name)
// 类实例是否响应指定的selector
BOOL class_respondsToSelector(Class cls, SEL sel)
// 添加协议
BOOL class_addProtocol(Class cls, Protocol *protocol)
// 为类添加属性
BOOL class_addProperty(Class cls, const char *name, const objc_property_attribute_t *attributes, unsigned int attributeCount)
// 替换类的属性
void class_replaceProperty(Class cls, const char *name, const objc_property_attribute_t *attributes, unsigned int attributeCount)
// 返回类是否实现指定的协议
BOOL class_conformsToProtocol(Class cls, Protocol *protocol)
// 返回类实现的协议列表
Protocol * __unsafe_unretained *class_copyProtocolList(Class cls, unsigned int *outCount)
// 获取版本号
int class_getVersion(Class theClass)
// 设置版本号
void class_setVersion(Class theClass, int version)
// 获取CoreFoundation的免费桥接
Class objc_getFutureClass(const char *name) // Do not call this function yourself.
// 设置CoreFoundation的免费桥接
void objc_setFutureClass(Class cls, const char *name) // Do not call this function yourself.
```

##1.2 Adding Classes

```objc
// 创建一个新类和元类
Class objc_allocateClassPair(Class superclass, const char *name, size_t extraBytes)
// 销毁一个类及其相关联的类
void objc_disposeClassPair(Class cls)
// 在应用中注册由objc_allocateClassPair创建的类
void objc_registerClassPair(Class cls)
// Do not call this function yourself.
Class objc_duplicateClass(Class original, const char *name, size_t extraBytes)
```

##1.3 Instantiating Classes

```objc
// 创建类实例
id class_createInstance(Class cls, size_t extraBytes)
// 在指定位置创建类实例
id objc_constructInstance(Class cls, void *bytes)
// 销毁类实例
void objc_destructInstance(id obj)
```

##1.4 Working with Instances

```objc
// 返回指定对象的一份拷贝
id object_copy(id obj, size_t size)
// 释放指定对象占用的内存
id object_dispose(id obj)
// 修改类实例的实例变量的值
Ivar object_setInstanceVariable(id obj, const char *name, void *value)
// 获取对象实例变量的值
Ivar object_getInstanceVariable(id obj, const char *name, void **outValue)
// 返回指向给定对象分配的任何额外字节的指针
void *object_getIndexedIvars(id obj)
// 返回对象中实例变量的值
id object_getIvar(id object, Ivar ivar)
// 设置对象中实例变量的值
void object_setIvar(id object, Ivar ivar, id value)
// 返回给定对象的类名
const char *object_getClassName(id obj)
// 返回对象的类
Class object_getClass(id object)
// 设置对象的类
Class object_setClass(id object, Class cls)
```

##1.5 Obtaining Class Definitions

```objc
// 获取已注册的类定义的列表
int objc_getClassList(Class *buffer, int bufferLen)
// 创建并返回一个指向所有已注册类的指针列表
Class *objc_copyClassList(unsigned int *outCount)
// 返回指定类的类定义
id objc_lookUpClass(const char *name)
// 返回指定类的类定义(会调用类处理程序)
id objc_getClass(const char *name)
// 返回指定类的类定义(会终止进程)
id objc_getRequiredClass(const char *name)
// 返回指定类的元类
id objc_getMetaClass(const char *name)
```

##1.6 Working with Instance Variables

```objc
// 获取成员变量名
const char * ivar_getName( Ivar ivar)
// 获取成员变量类型编码
const char * ivar_getTypeEncoding( Ivar ivar)
// 获取成员变量的偏移量
ptrdiff_t ivar_getOffset( Ivar ivar)
```

##1.7 Associative References

```objc
// 设置关联对象
void objc_setAssociatedObject(id object, void *key, id value, objc_AssociationPolicy policy)
// 获取关联对象
id objc_getAssociatedObject(id object, void *key)
// 移除关联对象
void objc_removeAssociatedObjects(id object)
```

##1.8 Sending Messages

```objc
// 消息转发 -> return id
id objc_msgSend(id self, SEL op, ...)
// objc_msgSend -> 返回一个double值
double objc_msgSend_fpret(id self, SEL op, ...)
// objc_msgSend -> 返回一个结构体
void objc_msgSend_stret(void * stretAddr, id theReceiver, SEL theSelector, ...)
// 发送消息到父类 -> return id
id objc_msgSendSuper(struct objc_super *super, SEL op, ...)
// objc_msgSend -> 返回一个结构体
void objc_msgSendSuper_stret(struct objc_super *super, SEL op, ...)
```

##1.9 Working with Methods

```objc
// 调用指定方法的实现
id method_invoke(id receiver, Method m, ...)
// 调用返回一个数据结构的方法的实现
void method_invoke_stret(id receiver, Method m, ...)
// 获取方法名
SEL method_getName( Method method)
// 返回方法的实现
IMP method_getImplementation( Method method)
// 获取描述方法参数和返回值类型的字符串
const char * method_getTypeEncoding( Method method)
// 获取方法的返回值类型的字符串
char * method_copyReturnType( Method method)
// 获取方法的指定位置参数的类型字符串
char * method_copyArgumentType( Method method, unsigned int index)
// 通过引用返回方法的返回值类型字符串
void method_getReturnType( Method method, char *dst, size_t dst_len)
// 返回方法的参数的个数
unsigned method_getNumberOfArguments( Method method)
// 通过引用返回方法指定位置参数的类型字符串
void method_getArgumentType( Method method, unsigned int index, char *dst, size_t dst_len)
// 返回指定方法的方法描述结构体
struct objc_method_description *method_getDescription( Method m)
// 设置方法的实现
IMP method_setImplementation( Method method, IMP imp)
// 交换两个方法的实现
void method_exchangeImplementations( Method m1, Method m2)
```

##1.10 Working with Libraries

```objc
// 获取所有加载的Objective-C框架和动态库的名称
const char **objc_copyImageNames(unsigned int *outCount)
// 获取指定类所在动态库
const char *class_getImageName(Class cls)
// 获取指定库或框架中所有类的类名
const char **objc_copyClassNamesForImage(const char *image, unsigned int *outCount)
```

##1.11 Working with Selectors

```objc
// 返回给定选择器指定的方法的名称
const char* sel_getName(SEL aSelector)
// 在Objective-C Runtime系统中注册一个方法，将方法名映射到一个选择器，并返回这个选择器
SEL sel_registerName(const char *str)
// 在Objective-C Runtime系统中注册一个方法
SEL sel_getUid(const char *str)
// 比较两个选择器
BOOL sel_isEqual(SEL lhs, SEL rhs)
```

>sel_registerName函数：在我们将一个方法添加到类定义时，我们必须在Objective-C Runtime系统中注册一个方法名以获取方法的选择器。

##1.12 Working with Protocols

```objc
// 返回指定的协议
Protocol *objc_getProtocol(const char *name)
// 获取运行时所知道的所有协议的数组
Protocol **objc_copyProtocolList(unsigned int *outCount)
// 创建新的协议实例
Protocol *objc_allocateProtocol(const char *name)
// 在运行时中注册新创建的协议
void objc_registerProtocol(Protocol *proto)
// 为协议添加方法
void protocol_addMethodDescription(Protocol *proto, SEL name, const char *types, BOOL isRequiredMethod, BOOL isInstanceMethod)
// 添加一个已注册的协议到协议中
void protocol_addProtocol(Protocol *proto, Protocol *addition)
// 为协议添加属性
void protocol_addProperty(Protocol *proto, const char *name, const objc_property_attribute_t *attributes, unsigned int attributeCount, BOOL isRequiredProperty, BOOL isInstanceProperty)
// 返回协议名
const char *protocol_getName(Protocol *p)
// 测试两个协议是否相等
BOOL protocol_isEqual(Protocol *proto, Protocol *other)
// 获取协议中指定条件的方法的方法描述数组
struct objc_method_description *protocol_copyMethodDescriptionList(Protocol *p, BOOL isRequiredMethod, BOOL isInstanceMethod, unsigned int *outCount)
// 获取协议中指定方法的方法描述
struct objc_method_description protocol_getMethodDescription(Protocol *p, SEL aSel, BOOL isRequiredMethod, BOOL isInstanceMethod)
// 获取协议中的属性列表
objc_property_t * protocol_copyPropertyList(Protocol *protocol, unsigned int *outCount)
// 获取协议的指定属性
objc_property_t protocol_getProperty(Protocol *proto, const char *name, BOOL isRequiredProperty, BOOL isInstanceProperty)
// 获取协议采用的协议
Protocol **protocol_copyProtocolList(Protocol *proto, unsigned int *outCount)
// 查看协议是否采用了另一个协议
BOOL protocol_conformsToProtocol(Protocol *proto, Protocol *other)
```

##1.13 Working with Properties

```objc
// 获取属性名 
const char *property_getName(objc_property_t property)
// 获取属性特性描述字符串
const char *property_getAttributes(objc_property_t property)
// 获取属性中指定的特性
char *property_copyAttributeValue(objc_property_t property, const char *attributeName)
// 获取属性的特性列表
objc_property_attribute_t *property_copyAttributeList(objc_property_t property, unsigned int *outCount)
```

##1.14 Using Objective-C Language Features

```objc
// 通过在一个foreach循环中检测到突变的编译器插入。
void objc_enumerationMutation(id obj)
// 设置突变处理
void objc_setEnumerationMutationHandler(void (*handler)(id))
// 创建一个指针函数的指针，该函数调用时会调用特定的block
IMP imp_implementationWithBlock(id block)
// 返回与IMP(使用imp_implementationWithBlock创建的)相关的block
id imp_getBlock( IMP anImp)
// 解除block与IMP(使用imp_implementationWithBlock创建的)的关联关系，并释放block的拷贝
BOOL imp_removeBlock( IMP anImp)
// 加载弱引用指针引用的对象并返回
id objc_loadWeak(id *location)
// 存储__weak变量的新值
id objc_storeWeak(id *location, id obj)
```

#2 Data Types

##2.1 Class-Definition Data Structures

```objc
// Objective-C类是由Class类型来表示的，实际上是一个指向objc_class结构体的指针
typedef struct objc_class *Class;
struct objc_class {
    Class isa  OBJC_ISA_AVAILABILITY;
 
#if !__OBJC2__
    Class super_class                       OBJC2_UNAVAILABLE;  // 父类
    const char *name                        OBJC2_UNAVAILABLE;  // 类名
    long version                            OBJC2_UNAVAILABLE;  // 类的版本信息，默认为0
    long info                               OBJC2_UNAVAILABLE;  // 类信息，供运行期使用的一些位标识
    long instance_size                      OBJC2_UNAVAILABLE;  // 该类的实例变量大小
    struct objc_ivar_list *ivars            OBJC2_UNAVAILABLE;  // 该类的成员变量链表
    struct objc_method_list **methodLists   OBJC2_UNAVAILABLE;  // 方法定义的链表
    struct objc_cache *cache                OBJC2_UNAVAILABLE;  // 方法缓存
    struct objc_protocol_list *protocols    OBJC2_UNAVAILABLE;  // 协议链表
#endif
 
} OBJC2_UNAVAILABLE;

// 类定义中的方法
typedef struct objc_method *Method;
struct objc_method {
    SEL method_name    OBJC2_UNAVAILABLE; // 方法名
    char *method_types OBJC2_UNAVAILABLE;
    IMP method_imp     OBJC2_UNAVAILABLE; // 方法实现
} 

// 实例变量的类型
typedef struct objc_ivar *Ivar;
struct objc_ivar {
    char *ivar_name  OBJC2_UNAVAILABLE; // 变量名
    char *ivar_type  OBJC2_UNAVAILABLE; // 变量类型
    int ivar_offset  OBJC2_UNAVAILABLE; // 基地址偏移字节
#ifdef __LP64__
    int space		    OBJC2_UNAVAILABLE; // 
#endif
}

/// An opaque type that represents a category.
typedef struct objc_category *Category;
struct objc_category {
    char *category_name                       OBJC2_UNAVAILABLE; // 分类名
    char *class_name                          OBJC2_UNAVAILABLE; // 分类所属的类名
    struct objc_method_list *instance_methods OBJC2_UNAVAILABLE; // 实例方法列表
    struct objc_method_list *class_methods    OBJC2_UNAVAILABLE; // 类方法列表
    struct objc_protocol_list *protocols      OBJC2_UNAVAILABLE; // 分类所实现的协议列表
} OBJC2_UNAVAILABLE;

Objective-C声明的属性的类型
typedef struct objc_property *objc_property_t;

// 函数指针，指向方法实现的首地址
id (*IMP)(id, SEL, ...)

// 选择器，是表示一个方法的selector的指针
typedef struct objc_selector *SEL;

// 定义了一个Objective-C方法
struct objc_method_description {
	SEL name;               /**< The name of the method */
	char *types;            /**< The types of the method arguments */
};

struct objc_method_list { struct objc_method_list *obsolete; int method_count; struct objc_method method_list[1]; }

// 缓存条用过的方法
typedef struct objc_cache *Cache OBJC2_UNAVAILABLE;
struct objc_cache {
    unsigned int mask /* total = mask + 1 */ OBJC2_UNAVAILABLE; // 一个整数，指定分配的缓存bucket的总数
    unsigned int occupied                    OBJC2_UNAVAILABLE; // 一个整数，指定实际占用的缓存bucket的总数
    Method buckets[1]                        OBJC2_UNAVAILABLE; // 指向Method数据结构指针的数组
};

// 协议列表
struct objc_protocol_list {
    struct objc_protocol_list *next;
    long count;
    Protocol *list[1];
};

typedef struct {
    const char *name;  /**< The name of the attribute */
    const char *value; /**< The value of the attribute (usually empty) */
} objc_property_attribute_t; // 属性的特性(attribute)
```

##2.2 Instance Data Types

```objc
// 任何oc对象
typedef struct objc_object *id;

// 表示一个类的实例的结构体
struct objc_object {
    Class isa  OBJC_ISA_AVAILABILITY; // 类的isa指针
};

// 表示一个父类的结构体
struct objc_super {
    /// Specifies an instance of a class.
    __unsafe_unretained id receiver; // 消息的实际接收者
    /// Specifies the particular superclass of the instance to message. 
#if !defined(__cplusplus)  &&  !__OBJC2__
    /* For compatibility with old objc-runtime.h header */
    __unsafe_unretained Class class;
#else
    __unsafe_unretained Class super_class; // 指针当前类的父类
#endif
    /* super_class is the first class to search */
};
```

##2.3 Boolean Value

```objc
// YES or NO (非0,非nil皆为真)
typedef signed char BOOL;
```

##2.4 Associative References

```objc
typedef OBJC_ENUM(uintptr_t, objc_AssociationPolicy) {
    OBJC_ASSOCIATION_ASSIGN = 0,           /**< Specifies a weak reference to the associated object. */
    OBJC_ASSOCIATION_RETAIN_NONATOMIC = 1, /**< Specifies a strong reference to the associated object. The association is not made atomically. */
    OBJC_ASSOCIATION_COPY_NONATOMIC = 3,   /**< Specifies that the associated object is copied. The association is not made atomically. */
    OBJC_ASSOCIATION_RETAIN = 01401,       /**< Specifies a strong reference to the associated object. The association is made atomically. */
    OBJC_ASSOCIATION_COPY = 01403          /**< Specifies that the associated object is copied. The association is made atomically. */
};
```

&#160;

----------

#Appendix

##Related Documentation

[Objective-C Runtime Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html)

[Objective-C Runtime 运行时之一：类与对象](http://www.cocoachina.com/ios/20141031/10105.html)

[Objective-C Runtime 运行时之二：成员变量与属性](http://www.cocoachina.com/ios/20141105/10134.html)

[Objective-C Runtime 运行时之三：方法与消息](http://www.cocoachina.com/ios/20141106/10150.html)

[Objective-C Runtime 运行时之四：Method Swizzling](http://www.cocoachina.com/ios/20140225/7880.html)

[Objective-C Runtime 运行时之五：协议与分类](http://www.cocoachina.com/ios/20141110/10174.html)

[Objective-C Runtime 运行时之六：拾遗](http://southpeak.github.io/blog/2014/11/09/objective-c-runtime-yun-xing-shi-zhi-liu-:shi-yi/)

[【OC刨根问底】-Runtime简单粗暴理解](http://www.jianshu.com/p/f900de4a1495)

[OC最实用的runtime总结，面试、工作你看我就足够了！](http://www.jianshu.com/p/ab966e8a82e2)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-07-27 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974