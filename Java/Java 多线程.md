# 1 简介

多线程编程是以线程为基本单位的一种编程范式，也可以理解为使用多个线程分工处理一个大型任务。

## 1.1 串行、并发与并行

在多线程中有三种概率串行、并发与并行。

1. 串行就是 A 完成了再执行 B 的操作。可以理解为线程 B 的执行依赖线程 A 的结束。
2. 并发就是 A B 同时进行，有一定的交集，但不是同时开始。
3. 并行是一种理想状态，它是指 A 和 B 同时开始执行任务。

## 1.2 Thread

在 Java 的多线程开发中核心类是 Thread, Thread 实现了 Runnable 接口。

```java
package java.lang;

@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
```

Runnable 只有一个run接口。实际开发中，我们是通过继承它实现一个任务，然后挂靠到 Thread 中执行，如下所示。

```java
Thread thread = new Thread(() -> {
    // 执行任务
});
thread.start();
```

> 实际开发中我们会通过线程池完成相关多线程开发

Thread 中常用的属性和方法如下所示：

```java
private long tid; // 线程ID
private volatile String name; // 线程名
private boolean daemon = false; // 是否守护线程，守护线程可以理解为用户线程，如垃圾回收线程。所有非守护线程结束，程序会结束。
private int priority; // 线程优先级[1,10],默认5

// 获取当前线程
public static native Thread currentThread();
// 使当前线程放弃对处理器的占用，会导致当前线程被暂停
public static native void yield();
// 使当前线程休眠指定的时间
public static native void sleep(long millis) throws InterruptedException;

// 线程启动
public synchronized void start()
// 线程停止
public void interrupt()
// 获取线程状态
public State getState()
// 暂停当前线程，等待目标线程执行完毕再执行当前线程
public final void join() throws InterruptedException
```

## 1.3 线程的生命周期

线程从创建、启动到其运行的结束会经历若干状态由 Thread.State 定义。

```java
public enum State {
    NEW, // 创建线程未启动时的状态
    RUNNABLE,// 线程运行
    BLOCKED, // 线程堵塞状态，不会占用处理器资源。如申请锁
    WAITING, // 线程等待其他线程的激活。如wait(),join()方法
    TIMED_WAITING, // 线程在一定的时间内等待其他线程的激活，
    TERMINATED;// 线程结束
}
```

## 1.4 线程的上下文切换

在一个处理器上运行多个线程时会发生上下文切换。如 A 执行完毕，接着执行 B 线程，此时处理器会加载 B 所需的资源从而产生上下文切换。线程对处理器的占用和释放都会产生线程的上下文切换问题。

线程的上下文切换有主动引起的和被动引起。如 Thread.sleep() 就会引起主动的切换，垃圾回收机制会引起被动的上下文切换。

## 1.5 线程的安全

线程的安全主要涉及三个方面

1. 原子性：线程内的操作对线程外来说是不可分割的。
2. 可见性：线程内对线程外的数据（共享）操作，能够被其他线程访问。
3. 有序性：程序的执行顺序和我们预期的源代码执行顺序是一致的。

如果违背了上面的三个要素，则会产生竞态问题。竞态是指程序的输出有时候是正确的有时候是错误的。如 1 + B，在执行 + 之前 B 是 1 ，但是操作过程中其他线程对 B 修改为2 那么，输出的结果就变成了错误的结果 3 ，产生了竞态。

为了解决竞态，并保证原子性、可见性和有序性我们增加了锁的操作。不合理的使用锁以及其他操作会引起线程的活性故障。

1. 死锁：两个或更多的线程因相互等待对方而被永远暂停。
2. 锁死：等待线程由于唤醒其所需的条件永远无法成立。
3. 活锁：线程一直处于运行状态，但是其任务却一直无法进展。
4. 饥饿：线程一直无法获得其所需的资源而导致其任务一直无法进展。

# 2 同步机制

锁的底层是借用了内存屏障。内存屏障是被插入到两个指令之间进行使用的，其作用是禁止编译器、处理器重排序从而保障有序性。

## 2.1 final 与 volatile

final 关键字的作用：保障对象不可重新赋值和类不可被继承。volatile 关键字的作用：保障可见性、有序性和 long/double 变量读写操作的原子性。

> 在 java 中除了 long/double 以外的变量写操作都是原子操作。

final 与 volatile 在一定程度保障了线程安全，但不是绝对的安全。如同时修改 final 引用对象内部值，将一个非原子性的操作结果赋值给 volatile 对象，也会产生原子性问题。为保障线程安全我们更多的是使用锁。

## 2.2 内部锁：synchronized

synchronized 是一种内部锁，它使用方便。可对方法和代码块加锁。

```java
// 方法加锁
public synchronized void test() { }

// 块加锁
synchronized (锁对象) { }
```

## 2.3 显示锁：Lock

### 2.3.1 ReentrantLock

显示锁（ReentrantLock） 和 内部锁不同，它是java.util.concurrent.locks.Lock 接口的实例。显示锁使得我们可以在一个方法内加锁，在其他方法内解锁。

```java
private final Lock lock = new ReentrantLock();

public void test() {
    lock.lock(); // 加锁
    try {

    } finally {
        lock.unlock(); // 解锁
    }
    
    if (lock.tryLock()) { // 试加锁，加锁失败立即返回false 不会等待
        try {
            // manipulate protected state
        } finally {
            lock.unlock();
        }
    } else {
        // perform alternative actions
        System.out.println("1");
    }

    try {
        if (lock.tryLock(1, TimeUnit.SECONDS)) { // 1秒内加锁，超时会返回false
            try {
                // manipulate protected state
            } finally {
                lock.unlock();
            }
        } else {
            // perform alternative actions
        }
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

### 2.3.2 ReadWriteLock

ReadWriteLock（读写锁） 是 Lock 的改进型锁，主要用于多读少写的操作。多个线程可以同时获得读锁，写锁只能一个线程获取。获取写锁时其他线程不能有读锁的占用，具体例子如下所示。

```java
public class ReadWriteLockTest {

    private final ReadWriteLock rwLock = new ReentrantReadWriteLock();
    private final Lock readLock = rwLock.readLock();
    private final Lock writeLock = rwLock.writeLock();

    public void read () {
        readLock.lock();
        try {
        } finally {
            readLock.unlock();
        }
    }

    public void write () {
        writeLock.lock();
        try {
        } finally {
            writeLock.unlock();
        }
    }

}
```

内部锁和显示锁是在同一个线程内加解锁的。后面将介绍不同线程加解锁，也可以理解为线程间的协作。

## 2.4 等待与通知：wait/notify

wait() 方法会暂停当前线程，等待其他线程调用 notify() 方法唤醒当前线程，如下所示。

```java
while (保护条件不成立) {
	wait();
}
```

notify() 只会唤醒一个等待线程，使用 notifyAll() 可以唤醒所有等待线程。但是这里会产生过早唤醒问题，如果 ab 都等待状态，c唤醒了ab，由于 b 的保护条件不成立，会又一次睡眠等待，这次无意义的唤醒就是过早唤醒。


## 2.5 Java条件变量：Condition

wait、notify 偏底层，且有过早唤醒问题，Java 提供了条件变量 Condition。通过不同的条件唤醒不同的线程。

```java
private final Lock lock = new ReentrantLock();
private final Condition condition1 = lock.newCondition();
private final Condition condition2 = lock.newCondition();

public void test() {
    lock.lock();
    try {
        while (保护条件1) {
            condition1.await();
        }
        while (保护条件2) {
            condition2.await();
        }
        // condition1.notifyAll(); 激活条件1的等待线程
    } finally {
      lock.unlock();
    }
}
```

## 2.6 倒计时协调器：CountDownLatch

CountDownLatch 可以用来实现一个或多个线程等待其他线程完成一组特定的操作之后才继续运行。CountDownLatch 内维护了一个计时器，当值为0时，所有 await() 线程可以接着往下运行。

```java
CountDownLatch latch = new CountDownLatch(2);
new Thread(() -> {
    try {
        latch.await();
        System.out.println("1");
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}).start();
latch.countDown(); //-1
System.out.println("0");
latch.countDown(); // -1, 此时计数器为0，await线程恢复执行
```

## 2.7 删栏：CyclicBarrier

CyclicBarrier 和 CountDownLatch 不同，它主要是用于多个线程相互等待对方执行到代码中的某个地方（集合点），然后这些线程才能够继续执行。内部也有一个计时器，当达到0时，所有 await 线程会继续执行。且它可以重复使用。await()方法即是减-，又有等待的作用。

```java
CyclicBarrier cyclicBarrier = new CyclicBarrier(2);
new Thread(() -> {
    try {
        cyclicBarrier.await(); // -1
        System.out.println("1");
    } catch (Exception e) {
        e.printStackTrace();
    }
}).start();
try {
    cyclicBarrier.await(); // -1
    System.out.println("0");
} catch (Exception e) {
    e.printStackTrace();
}
```

# 3 线程管理

## 3.1 线程组：ThreadGroup

线程组（ThreadGroup）可以用来表示一组相似的线程。线程与线程组之间的关系类似于文件与文件夹的关系，如果一个线程的创建没有指定线程组，那么它的线程组默认为创建方的线程组。

ThreadGroup 实现了 Thread.UncaughtExceptionHandler 接口，通过它我们可以实现对线程的异常捕获与监控。

## 3.2 线程工厂：

线程工厂（ThreadFactory）给我们提供了工厂的方式使用 newThread 方法创建定制的接口，这使得我们的线程很多相同的设置可以流水线生产。

```java
public interface ThreadFactory {
    Thread newThread(Runnable r);
}
```

## 3.3 线程池

在一个项目中线程的使用是有一点的开销，如果不合理管理，反而会降低系统的性能。线程池合理管理线程的使用，内部可以预先创建一定数量的工作者线程，客户端只需要将执行的任务作为一个对象提交给线程池，线程池将这些任务缓存在队列中，而线程池内部的各个工作者线程则不断地从队列中取出任务并执行。线程池可以被看作是基于生产者-消费者模式的一种服务。

### 3.3.1 ThreadPoolExecutor

ThreadPoolExecutor 是线程池的核心类。内部有核心线程、工作线程和最大线程数。它们满足这样的关系:(核心线程池数、工作池线程数<=最大线程池数）

```java
ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(0, 2, 4, TimeUnit.SECONDS, new ArrayBlockingQueue<Runnable>(100));
// 添加任务
Future<String> future = threadPoolExecutor.submit(() -> {
    Thread.sleep(5000);
    return "YJ";
});
// 获取线程返回结果
try {
    System.out.println(future.get()); // 堵塞当前线程等待目标线程返回结果
} catch (Exception e) {
    e.printStackTrace();
}
```

WorkQueue 队列是有限的，如果我们把队列提交满时，再次提交数据则会报 RejectedExecutionException 错误。

不合理的使用线程池也会产生死锁问题，如线程池的当前线程向线程池提交一个任务并需要立即获取数据，而提交的任务无法被线程池提供线程执行，这种等待现象会一直持续下去，从而产生死锁。

### 3.3.2 ScheduledThreadPoolExecutor

在有些情况，我们可能需要事先提交一个任务，这个任务不是立即执行的，而是在特定的时间执行或周期性的执行则需要使用 ScheduledThreadPoolExecutor 。ScheduledThreadPoolExecutor 继承了 ThreadPoolExecutor 的一切特性。

```java
SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
ScheduledThreadPoolExecutor pool = new ScheduledThreadPoolExecutor(5);
pool.schedule(() -> { // 延时5秒执行
    System.out.println("1-" + formatter.format(new Date()));
}, 5, TimeUnit.SECONDS);
pool.scheduleAtFixedRate(() -> { // 延迟2秒后周期1秒执行任务
    System.out.println("2-" + formatter.format(new Date()));
}, 2,1, TimeUnit.SECONDS);
System.out.println("开始-" + formatter.format(new Date()));
```

### 3.3.3 ForkJoinPool

ThreadPoolExecutor 和 ScheduledThreadPoolExecutor 当线程池任务队列满的时候，都无法添加任务，且任务之间的关系是平级的，没有父子之分。ForkJoinPool 支持添加无限的任务，且内部支持任务父子关系，不会产生死锁问题。

ForkJoinPool 主要用于将一个大任务拆分到多个子线程去完成，这里使用 ForkJoinPool 实现了快速排序算法。

```java
public class ForkJoinPoolTest {

    private final ForkJoinPool pool = new ForkJoinPool();

    public void quickSort(List<Integer> list) {
        if (list == null || list.size() <= 1)
            return;
        try {
            pool.submit(() -> { // 提交总任务
                quiceSort(list, 0, list.size() - 1);
            }).get();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void quiceSort(List<Integer> list, int start, int end) {
        if (start >= end)
            return;
        int index = this.partition(list, start, end);
        if (end - start >= 10) { // 拆分子任务
            ForkJoinTask left = new RecursiveTask() {
                protected Object compute() {
                    quiceSort(list, start, index - 1);
                    return null;
                }
            };
            left.fork();
            ForkJoinTask right = new RecursiveTask() {
                protected Object compute() {
                    quiceSort(list, index + 1, end);
                    return null;
                }
            };
            right.fork();
            left.join();
            right.join();
        } else {
            quiceSort(list, start, index - 1);
            quiceSort(list, index + 1, end);
        }
    }

    private int partition(List<Integer> list, int start, int end) {
        int index = start;
        int last = list.get(end);
        for (int i = start; i < end; i++) {
            if (list.get(i) <= last) {
                this.exchange(list, index, i);
                index++;
            }
        }
        this.exchange(list, index, end);
        return index;
    }

    private void exchange(List<Integer> list, int start, int end) {
        if (start == end)
            return;
        int temp = list.get(start);
        list.set(start, list.get(end));
        list.set(end, temp);
    }

    public static void main(String[] args) {
        SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println(formatter.format(new Date()));
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < 10000000; i++) {
            list.add((int) (Math.random() * 1000000));
        }
        ForkJoinPoolTest test = new ForkJoinPoolTest();
        test.quickSort(list);
        System.out.println(formatter.format(new Date()));
        for (int i = 0; i < list.size() - 2; i++) {
            if (list.get(i) > list.get(i+1)) {
                System.out.println("排序失败");
            }
        }
        System.out.println(formatter.format(new Date()));
    }

}
```
&#160;

----------

# Appendix

## Related Documentation

[Java 多线程编程实战指南](https://detail.tmall.com/item.htm?id=551006732361&spm=a1z09.2.0.0.1dca79deOamT0K&_u=mimv3anab13)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-11-23 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974