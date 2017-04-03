#1 性能对比

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017031601.png)

#2 介绍与使用

##2.1 OSSpinLock

OSSpinLock 目前已不再安全，主要原因发生在低优先级线程拿到锁时，高优先级线程进入忙等(busy-wait)状态，消耗大量 CPU 时间，从而导致低优先级线程拿不到 CPU 时间，也就无法完成任务并释放锁。这种问题被称为优先级反转。

```objc
OSSpinLock theLock = OS_SPINLOCK_INIT;
OSSpinLockLock(&theLock);
OSSpinLockUnlock(&theLock);
```

##2.2 dispatch_semaphore

dispatch_semaphore 是 GCD 用来同步的一种方式。允许通过 wait/signal 的信号事件控制并发执行的最大线程数，当最大线程数降级为1的时候则可当作同步锁使用。**注意该信号量并不支持递归**

```objc
dispatch_semaphore_t signal = dispatch_semaphore_create(1);
dispatch_semaphore_wait(signal, overTime);
dispatch_semaphore_signal(signal);
```

##2.3 pthread_mutex

POSIX标准的unix多线程库(pthread)中使用的互斥量，支持 while 循环。

```objc
pthread_mutex_t lock
pthread_mutex_init(&lock, NULL);
pthread_mutex_lock(&lock);加锁
pthread_mutex_tylock(&lock); / pthread_mutex_trylock(&lock) == 0
pthread_mutex_unlock(&lock);
pthread_mutex_destroy(&lock);
```

##2.4 NSLock

典型的面向对象的锁，即同步锁类。所有锁（包括NSLock）遵循 Objective-C 的 NSLocking 协议接口，支持 tryLock 和 lockBeforeDate: 。底层通过 pthread_mutex 实现。

```objc
NSLock *lock = [[NSLock alloc] init];

if ([lock tryLock]) { // 尝试加锁，如果失败返回NO，不会阻塞该线程
	[lock unlock];
}
if ([lock lockBeforeDate:date]) { // 在Date之前尝试加锁，如果不能加锁，则返回NO
	[lock unlock];
}
```

##2.5 NSCondition

NSCondition基于信号量方式实现的锁对象，提供单独的信号量管理接口。底层通过pthread_cond_t实现。

```objc
NSCondition *condition = [[NSCondition alloc] init];
[condition lock];
[condition wait];   // 让当前线程处于等待状态
[condition signal]; // CPU发信号告诉线程不用在等待，可以继续执行
[condition unlock];
```

##2.6 pthread_mutex(recursive)

pthread_mutex的递归锁实现，作用和NSRecursiveLock类似，主要为了防止在递归的情况下出现死锁。

```objc
pthread_mutex_t lock;

pthread_mutexattr_t attr;
pthread_mutexattr_init(&attr);
pthread_mutexattr_settype(&attr, PTHREAD_MUTEX_RECURSIVE);
pthread_mutex_init(&lock, &attr);
pthread_mutexattr_destroy(&attr);

pthread_mutex_lock(&lock);
pthread_mutex_unlock(&lock);

pthread_mutex_destroy(&lock);
```

##2.7 NSRecursiveLock递归锁

NSRecursiveLock 和 NSLock 一样是面向对象的锁，但是支持递归，不支持tryLock。这个锁可以被同一线程多次请求，而不会引起死锁。这主要是用在循环或递归操作中。

```objc
NSRecursiveLock *theLock = [[NSRecursiveLock alloc] init];
  
void RecursiveFunction(int value) {
    [theLock lock];
    if (value != 0) {
        --value;
        RecursiveFunction(value);
    }
    [theLock unlock];
}
  
RecursiveFunction(5);
```

##2.8 NSConditionLock条件锁

NSConditionLock 可以使用特定值来加锁和解锁。相比NSCondition更为直接和实用。

```
id condLock = [[NSConditionLock alloc] initWithCondition:NO_DATA];
while(true) {
    [condLock lock];
    /* Add data to the queue. */
    [condLock unlockWithCondition:HAS_DATA];
}
```

##2.9 @synchronized

通过 @synchronized 可以非常方便的创建一个互斥锁非常方便的方法。@synchronized 通过对象的哈希表来实现。

```objc
- (void)myMethod:(id)anObj {
    @synchronized(anObj) {
    
    }
}
```

#3 结论

1. 常规和 while 循环加锁使用 pthread_mutex。
2. 队列（dispatch_get_global_queue）并发线程数控制，使用 dispatch_semaphore。
3. 递归调用时使用 NSRecursiveLock（项目中不建议使用此方案，风险高）。
4. 性能要求极度苛刻时，考虑使用 OSSpinLock，不过需要确保加锁片段的耗时足够小。
6. 不建议使用 NSLock、pthread_mutex(recursive)、NSCondition、NSConditionLock 和 @synchronized。

> 其实除了上面9种锁，我们还可以通过串行队列实现同步锁。不过那种感觉就像拿着火炮打蚊子，打的准不准不说，其打的速度堪忧。

&#160;

----------

#Appendix

##Related Documentation

[不再安全的 OSSpinLock](http://blog.ibireme.com/2016/01/16/spinlock_is_unsafe_in_ios/)

[深入理解 iOS 开发中的锁](http://www.jianshu.com/p/8781ff49e05b)

[起底多线程同步锁(iOS)](http://www.cocoachina.com/ios/20160129/15170.html)

[[iOS] 正确使用多线程同步锁 @synchronized()](http://www.tuicool.com/articles/b2QB7vu)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-03-16 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974