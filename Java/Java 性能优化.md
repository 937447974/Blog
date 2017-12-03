Java 性能优化主要是通过 jconsole（java监视与管理控制台）和命令行（top、jstack）分析程序的运行状态，找到有问题的代码并修复。

# 1 代码

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017120301.png)

运行上面的测试代码，我们很明显的知道错误代码是在 25 行左右，会形成死循环，引起 CPU 过高。接下来我们通过工具的方式找到问题代码。

# 2 命令行分析

进入命令行执行 `top` 命令，可以看见如下所示界面

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017120302.png)

pid=9665 的 cpu 耗用非常高，执行命令 `jstack 9665 > java.log` 命令抓取日志信息并写入 java.log 文件中。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017120305.png)

通过文件我们发现问题出在 JavaTest.java 的第25行命令。

# 3 jconsole 分析

使用 top 和 jstack 循环抓取可分析项目中有问题的代码，但这种方式并不是很高效。Java 给我们提供了基于 JMX 的可视化监视管理工具 jconsole。

命令行执行 `jconsole `，选中需要分析的项目，即可看见如下面板

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017120303.png)

这里我们可以观察到很多信息，日常工作中我们也多是通过这种方式分析监控项目。点击线程模块。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017120304.png)

这里可以通过 main 线程很明显的发现问题，主线程一直停留在25行代码。

&#160;

----------

# Appendix

## Related Documentation

[Using JConsole](https://docs.oracle.com/javase/9/management/using-jconsole.htm#JSMGM-GUID-77416B38-7F15-4E35-B3D1-34BFD88350B5)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-12-03 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974