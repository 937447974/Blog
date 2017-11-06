java 开发过程中，我们可以通过 ide 和 命令行分析程序的运行状态。这里主要介绍通过命令行分析程序的性能。

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

这里我的电脑是 mac 系统。进入命令行执行 `top` 命令，可以看见如下所示界面。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation


## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-10-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974