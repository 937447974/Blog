#1 RAM&ROM

RAM与ROM就是具体的存储空间，统称为存储器。

1. RAM(random access memory)：运行内存，CPU可以直接访问，读写速度非常快，但是不能掉电存储。它又分为：
	1. 动态DRAM，速度慢一点，需要定期的刷新(充电)，我们常说的内存条就是指它，价格会稍低一点，手机中的运行内存也是指它。
	2. 静态SRAM，速度快，我们常说的一级缓存，二级缓存就是指它，当然价格高一点。
2. ROM(read only memory)：存储性内存，可以掉电存储，例如SD卡。

#2 内存五区

1. 栈区（stack）
	- 栈区地址从高到低分配；
	- 存放的局部变量（先进后出）一旦出了作用域就会被销毁；
	- 大量的局部变量，深递归，函数循环调用都可能耗尽栈内存而造成程序崩溃 。
2. 堆区（heap）
	- 堆区的地址是从低到高分配。
	- ARC下OC对象runloop循环结束后(kCFRunLoopBeforeWaiting后)自动释放，CF对象需要`CFRelease(CFTypeRef cf)`手动释放。
3. 全局区／静态区（static）
	- 包括两个部分：未初始化过和初始化过。（全局区／静态区）在内存中是放在一起的，初始化的全局变量和静态变量在一块区域， 未初始化的全局变量和静态变量在相邻的另一块区域；
	` eg：int a;未初始化的。int a = 10;已初始化的。`
4. 常量区
	- 常量字符串就是放在这里，还有const常量；
5. 代码区
	- 存放App代码，4S(iOS7)只有74M；

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017031501.png)

#3 RAM&ROM协调工作

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017031502.png)


&#160;

----------

#Appendix

##Related Documentation、

[unix之内存管理](http://blog.csdn.net/tao546377318/article/details/51654993)

[linux内存管理](http://www.cnblogs.com/autum/archive/2012/10/12/linuxmalloc.html)

[C语言中内存分配](http://blog.csdn.net/youoran/article/details/10990815)

[深入浅出－iOS内存分配与分区](http://www.jianshu.com/p/7bbbe5d55440)

[iOS开发中的内存分配与分区](http://www.cocoachina.com/ios/20161009/17700.html)

[iOS之CF和OC之间类型转换](http://blog.csdn.net/annkey123/article/details/8271806)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-03-15 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974