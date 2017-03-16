Grand Central Dispatch (GCD)是Apple开发的一个多核编程的解决方法。

GCD包括语言特性、运行库和系统改进，提供系统的、全面的改进并支持在iOS和OS X多核硬件上执行并发代码。

在ARC模式下，派遣到GCD中的对象是自动释放的。

#1 Functions

##1.1 Creating and Managing Queues

```objc
// 主队列
dispatch_queue_t dispatch_get_main_queue(void);
// 获取全局并发队列
dispatch_queue_t dispatch_get_global_queue(long identifier, unsigned long flags);
// 创建一个串行队列
dispatch_queue_t dispatch_queue_create( const char *label dispatch_queue_attr_t attr);
// 返回通过指定服务质量信息创建的队列的属性信息
dispatch_queue_attr_t dispatch_queue_attr_make_with_qos_class(dispatch_queue_attr_t attr, dispatch_qos_class_t qos_class, int relative_priority);
//  当创建队列时指定的标签信息
const char * dispatch_queue_get_label(dispatch_queue_t queue);
// 为给定的对象绑定一个目标队列
void dispatch_set_target_queue(dispatch_object_t object, dispatch_queue_t queue);
// 执行已经被提交到主队列中的block
void dispatch_main(void);
```

##1.2 Queuing Tasks for Dispatch

GCD为应用程序提供和管理FIFO队列让你的程序可以以block对象的形式来提交你的任务。block提交给调度队列的任务在一个完全由系统管理的线程池中执行。无法保证哪个任务在哪个线程中执行。GCD提供三种类型的队列：

- Main主队列：任务在你的应用程序主线程中被挨个连续的执行。
- Concurrent并发队列：任务被按照FIFO的原则进行取出，但是却可以并发的运行着并可以任何顺序完成。
- Serial连续队列：按照FIFO原则取出并挨个执行，一次执行一个任务。

```objc
// 提交一个block到队列中，异步执行，方法会立刻放回
void dispatch_async(dispatch_queue_t queue, dispatch_block_t block);
// 提交一个APP内定义的方法到队列中异步的执行，方法立即返回
void dispatch_async_f(dispatch_queue_t queue, void *context, dispatch_function_t work);

// 提交一个block到一个队列中执行并且等待这个block被执行完毕。
void dispatch_sync(dispatch_queue_t queue, dispatch_block_t block);
// 提交一个APP内定义的方法到队列中执行并等待这个方法执行完毕
void dispatch_sync_f(dispatch_queue_t queue, void *context, dispatch_function_t work);

// 在指定的时间执行这个block
void dispatch_after(dispatch_time_t when, dispatch_queue_t queue, dispatch_block_t block);
// 在指定的时间执行这个程序定义的方法
void dispatch_after_f(dispatch_time_t when, dispatch_queue_t queue, void *context, dispatch_function_t work);

// 提交一个block到队列中，多次调用。
void dispatch_apply(size_t iterations, dispatch_queue_t queue, void (^block)(size_t));
// 提交一个程序定义的方法到队列中，多次调用
void dispatch_apply_f(size_t iterations, dispatch_queue_t queue, void *context, void (*work)(void *, size_t));

// 执行一次该block对象，并且在整个程序的生命周期内只执行这么一次
void dispatch_once(dispatch_once_t *predicate, dispatch_block_t block);
// 执行一次该程序定义的方法，并且在整个程序的生命周期内只执行这么一次
void dispatch_once_f(dispatch_once_t *predicate, void *context, dispatch_function_t function);
```

##1.3 Using Dispatch Groups

分组块允许聚合同步。你的应用可以提交多个block并跟踪它们的完成，甚至于它们在不同的队列中执行。

```objc
// 创建一个可关联block的组
dispatch_group_t dispatch_group_create(void);

// 异步在队列中执行block，并于指定的调度组关联起来
void dispatch_group_async(dispatch_group_t group, dispatch_queue_t queue, dispatch_block_t block);

// 异步在队列中执行应用中的方法，并于指定的调度组关联起来
void dispatch_group_async_f(dispatch_group_t group, dispatch_queue_t queue, void *context, dispatch_function_t work);

// 设置队列超时时间
long dispatch_group_wait(dispatch_group_t group, dispatch_time_t timeout);

// 组内代码执行完毕后执行block
void dispatch_group_notify(dispatch_group_t group, dispatch_queue_t queue, dispatch_block_t block);
// 组内代码执行完毕后执行应用内方法
void dispatch_group_notify_f(dispatch_group_t group, dispatch_queue_t queue, void *context, dispatch_function_t work);

// 明确指示一个新的block进入到了调度组
void dispatch_group_enter(dispatch_group_t group);

// 明确指示block在调度组中执行完毕
void dispatch_group_leave(dispatch_group_t group);
```

##1.4 Managing Dispatch Objects

GCD提供调度对象接口允许你的程序来管理诸如处理内存管理、暂停和恢复执行、定义对象环境和打印任务数据等。调度对象必须手动的持有retain和释放release，否则不会被回收。

```objc
// 调度对象增加引用计数
void dispatch_retain(dispatch_object_t object);
// 调度对象减少引用计数
void dispatch_release(dispatch_object_t object);

// 获取调度对象的应用程序上下文
void *dispatch_get_context(dispatch_object_t object);
// 设置调度对象的应用程序上下文
void dispatch_set_context(dispatch_object_t object, void *context);

// 设置调度对象的终结函数
void dispatch_set_finalizer_f(dispatch_object_t object, dispatch_function_t finalizer);

// 调度对象暂停
void dispatch_suspend(dispatch_object_t object);
// 调度对象恢复运行
void dispatch_resume(dispatch_object_t object);

// 设置对象超时时间
long dispatch_wait(void *object, dispatch_time_t timeout);

// 调度对象执行完毕的通知
void dispatch_notify(void *object, dispatch_object_t queue, dispatch_block_t notification_block);

// 取消执行
void dispatch_cancel(void *object);
// 判断能否取消执行
long dispatch_testcancel(void *object);
```

##1.5 Using Semaphores

调度信号是一个传统的计数信号量的高效实现，可用于实现同步代码。当线程需要堵塞运行时，调用信号会直接调用内核。

```objc
// 创建调度信号 value=0暂停、大于0执行
dispatch_semaphore_t dispatch_semaphore_create(long value);
// 递减计数信号量。如果得到的值小于零，该功能在FIFO为信号发生之前回来。并设置超时时间
long dispatch_semaphore_wait(dispatch_semaphore_t dsema, dispatch_time_t timeout\*DISPATCH_TIME_FOREVER*\);
// 增量计数信号量。如果先前的值小于零，该功能目前在dispatch_semaphore_wait唤醒一个线程
long dispatch_semaphore_signal(dispatch_semaphore_t dsema);
```

##1.6 Using Barriers

调度障碍允许在并发队列中设置一个同步点。在同步点之前的所有代码执行完毕时，同步点后的代码才会执行。

```objc
// 异步执行的障碍block
void dispatch_barrier_async(dispatch_queue_t queue, dispatch_block_t block);
// 异步执行的障碍函数
void dispatch_barrier_async_f(dispatch_queue_t queue, void *context, dispatch_function_t work);

// 同步执行的障碍block
void dispatch_barrier_sync(dispatch_queue_t queue, dispatch_block_t block);
// 同步执行的障碍函数
void dispatch_barrier_sync_f(dispatch_queue_t queue, void *context, dispatch_function_t work);
```

##1.7 Managing Dispatch Sources

GCD提供了一系列接口用于检测GCD的相关运行状态。

```objc
// 创建监听源
dispatch_source_t dispatch_source_create(dispatch_source_type_t type, uintptr_t handle, unsigned long mask, dispatch_queue_t queue);

// 设置事件处理程序-block
void dispatch_source_set_event_handler(dispatch_source_t source, dispatch_block_t handler);
// 设置事件处理程序-函数
void dispatch_source_set_event_handler_f(dispatch_source_t source, dispatch_function_t handler);

// 取消事件处理程序-block
void dispatch_source_set_cancel_handler(dispatch_source_t source, dispatch_block_t handler);
// 取消事件处理程序-函数
void dispatch_source_set_cancel_handler_f(dispatch_source_t source, dispatch_function_t handler);

// 取消调度源
void dispatch_source_cancel(dispatch_source_t source);

// 判断调度源是否已取消
long dispatch_source_testcancel(dispatch_source_t source);

// 返回dispatch source管理的底层系统数据类型，即调用dispatch_source_create的第二个参数
uintptr_t dispatch_source_get_handle(dispatch_source_t source);
// 得到调用dispatch_source_create的第三个参数
unsigned long dispatch_source_get_mask(dispatch_source_t source);

// 获取dispatch_source_merge_data的第二个参数
unsigned long dispatch_source_get_data(dispatch_source_t source);
// 合并dispatch源数据，在dispatch源的block中.类型DISPATCH_SOURCE_TYPE_DATA_ADD or DISPATCH_SOURCE_TYPE_DATA_OR
void dispatch_source_merge_data(dispatch_source_t source, unsigned long value);

// 设置定时器
void dispatch_source_set_timer(dispatch_source_t source, dispatch_time_t start, uint64_t interval, uint64_t leeway);

// 注册事件源启动监听，即调用dispatch_resume()方法时调用block
void dispatch_source_set_registration_handler(dispatch_source_t source, dispatch_block_t handler);
// 注册事件源启动监听，即调用dispatch_resume()方法时调用函数
void dispatch_source_set_registration_handler_f(dispatch_source_t source, dispatch_function_t handler);
```

##1.8 Using the Dispatch I/O Convenience API

调度I / O方便的接口，可以让您执行文件描述符的异步读写操作。此接口支持基于流的语义访问文件描述符的内容。

```objc
// 异步读取指定的描述符
void dispatch_read(dispatch_fd_t fd, size_t length, dispatch_queue_t queue, void (^handler)(dispatch_data_t data, int error));

// 异步写入指定的描述符
void dispatch_write(dispatch_fd_t fd, dispatch_data_t data, dispatch_queue_t queue, void (^handler)(dispatch_data_t data, int error));
```

##1.9 Using the Dispatch I/O Channel API

dispatch I/O 以流的方式管理文件的相关操作。

```objc
// 创建io通道
dispatch_io_t dispatch_io_create(dispatch_io_type_t type, dispatch_fd_t fd, dispatch_queue_t queue, void (^cleanup_handler)(int error));
// 通过路径创建IOS通道
dispatch_io_t dispatch_io_create_with_path(dispatch_io_type_t type, const char *path, int oflag, mode_t mode, dispatch_queue_t queue, void (^cleanup_handler)(int error));
// 通过io创建IO通道
dispatch_io_t dispatch_io_create_with_io(dispatch_io_type_t type, dispatch_io_t io, dispatch_queue_t queue, void (^cleanup_handler)(int error));

// 读取文件内容
void dispatch_io_read(dispatch_io_t channel, off_t offset, size_t length, dispatch_queue_t queue, dispatch_io_handler_t io_handler);
// 写入文件内容
void dispatch_io_write(dispatch_io_t channel, off_t offset, dispatch_data_t data, dispatch_queue_t queue, dispatch_io_handler_t io_handler);

// 关闭IO通道读或写的操作
void dispatch_io_close(dispatch_io_t channel, dispatch_io_close_flags_t flags);

// 添加io通道操作完毕的block回调
void dispatch_io_barrier(dispatch_io_t channel, dispatch_block_t barrier);

// 获取dispatch_io_create的第二个参数
dispatch_fd_t dispatch_io_get_descriptor(dispatch_io_t channel);

// io通道设置高优先级
void dispatch_io_set_high_water(dispatch_io_t channel, size_t high_water);

// io通道设置低优先级
void dispatch_io_set_low_water(dispatch_io_t channel, size_t low_water);

// 设置处理间隔
void dispatch_io_set_interval(dispatch_io_t channel, uint64_t interval, dispatch_io_interval_flags_t flags);
```

##1.10 Managing Dispatch Data Objects

调度数据对象提供一个接口来管理基于内存的数据缓冲区。客户端访问数据缓冲区时将它看做一个连续的内存块，但缓冲区在内部可能由多个不连续的内存块组成。

```objc
// 在指定的缓冲区创建调度对象
dispatch_data_t dispatch_data_create(const void *buffer, size_t size, dispatch_queue_t queue, dispatch_block_t destructor);

// 获取对象占用的缓冲区大小
size_t dispatch_data_get_size(dispatch_data_t data);

// 通过指定的缓存对象和缓存创建一个新的缓存对象
dispatch_data_t dispatch_data_create_map(dispatch_data_t data, const void **buffer_ptr, size_t *size_ptr);

// 合并缓存对象成新的缓存对象
dispatch_data_t dispatch_data_create_concat(dispatch_data_t data1, dispatch_data_t data2);

// 获取缓存对象指定位置的数据
dispatch_data_t dispatch_data_create_subrange(dispatch_data_t data, size_t offset, size_t length);

// 遍历对象
bool dispatch_data_apply(dispatch_data_t data, dispatch_data_applier_t applier);

// 查找包含指定对象所表示的区域之间的指定位置的连续内存区域，并返回表示该区域的内部调度数据对象的副本
dispatch_data_t dispatch_data_copy_region(dispatch_data_t data, size_t location, size_t *offset_ptr);
```

##1.11 Managing Time

```objc
// 生成时间=when+delta
dispatch_time_t dispatch_time(dispatch_time_t when, int64_t delta);

// 通过一个结构体计算绝对时间
dispatch_time_t dispatch_walltime(const struct timespec *when, int64_t delta);
```

##1.12 Managing Queue-Specific Context Data

```objc
// 为队列设置标示
void dispatch_queue_set_specific(dispatch_queue_t queue, const void *key, void *context, dispatch_function_t destructor);

// 获取指定队列标示
void *dispatch_queue_get_specific(dispatch_queue_t queue, const void *key);

// 获取当前队列标示
void *dispatch_get_specific(const void *key);
```

&#160;

----------

#Appendix

##Related Documentation

[Grand Central Dispatch (GCD) Reference](https://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/index.html#//apple_ref/c/tdef/dispatch_once_t)

[Concurrency Programming Guide](https://developer.apple.com/library/ios/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html)

[iOS学习篇之Grand Central Dispatch（GCD）](http://www.tuicool.com/articles/NnURNrI)

[GCD(Grand Central Dispatch)教程](http://www.dreamingwish.com/article/gcdgrand-central-dispatch-jiao-cheng.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-08-31 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974