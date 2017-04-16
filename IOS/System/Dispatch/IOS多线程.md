## 线程

```objective-c
dispatch_queue_t queue = dispatch_get_main_queue();// 主线程
queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_BACKGROUND, 0);// 后台执行
```

## 异步执行队列任务

```objective-c
dispatch_async(queue, ^{
    NSLog(@"开新线程执行");
});
```

## 延时执行

```objective-c
// 2s后执行
dispatch_time_t when = dispatch_time(DISPATCH_TIME_NOW, 2 * NSEC_PER_SEC);
dispatch_after(when, queue, ^{
    NSLog(@"dispatch_after");
});
[self performSelector:@selector(after) withObject:nil afterDelay:2];

- (void)after {
 NSLog(@"performSelector");
}
```

## 单例

```objective-c
// 多线程模式下只执行一次
static dispatch_once_t predicate;
dispatch_once(&predicate, ^{
    NSLog(@"dispatch_once");
});
```

## 分组执行

```objective-c
// 分组执行
queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_BACKGROUND, 0);// 默认优先级执行
dispatch_group_t group = dispatch_group_create();
for (int i = 0; i<10; i++) {
    //异步执行队列任务
    dispatch_group_async(group, queue, ^{
        NSLog(@"dispatch_group_async:%d", i);
    });
}
// 组执行完毕后执行
dispatch_group_notify(group, queue, ^{
    NSLog(@"dispatch_group_notify");
});

```

## 串发

```objc
// 串行队列：只有一个线程，加入到队列中的操作按添加顺序依次执行。
queue = dispatch_queue_create("yangj", DISPATCH_QUEUE_SERIAL);
for (int i = 0; i<10; i++) {
    //异步执行队列任务
    dispatch_async(queue, ^{
        NSLog(@"dispatch_queue_create:%d", i);
    });
}
```

## 并发

```objc
// 并发队列：有多个线程，操作进来之后它会将这些队列安排在可用的处理器上，同时保证先进来的任务优先处理。
queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
for (int i = 0; i<10; i++) {
    //异步执行队列任务
    dispatch_async(queue, ^{
        NSLog(@"dispatch_get_global_queue:%d", i);
    });
}
```

## 同步

```objc
int cpuCount = [[NSProcessInfo processInfo] processorCount];// 内核2倍锁
// value=0暂停、大于0执行
dispatch_semaphore_t jobSemaphore = dispatch_semaphore_create(cpuCount * 2);
dispatch_semaphore_wait(jobSemaphore, DISPATCH_TIME_FOREVER); // -1
dispatch_semaphore_signal(jobSemaphore); // +1
```

&#160;

----------

# 其他

## 参考资料

[Grand Central Dispatch (GCD) Reference](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/index.html)

[谈iOS多线程(NSThread、NSOperation、GCD)编程](http://www.cocoachina.com/ios/20160129/15153.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-09-23 | GCD编程 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog