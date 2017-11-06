java 开发过程中，我们可以通过 ide 和命令行分析程序的运行状态。这里主要介绍通过命令行分析程序的性能。

运行测试代码。

```java
public class JavaTest {

    @Test
    public void test() {
        Integer i = 0;
        while (true) {
            i = 1;
        }
    }

}
```

很明显这段代码会引起 CPU 过高。其他如读写，内存都可通过此方法分析。

这里我的电脑是 mac 系统。进入命令行执行 `top` 命令，可以看见如下所示界面

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017110601.png)

pid=25654 的cpu 耗用非常高，执行命令 `jstack 25654 > java.log` 命令抓取日志信息并写入 java.log 文件中。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017110602.png)

通过文件我们发现问题出在 JavaTest.java 的 第18行命令，查询源代码，发现第18行代码是 `i = 1;`，对照前后，发现是因为死循环引起 CPU 过高。

&#160;

----------

# Appendix


## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-11-06 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974