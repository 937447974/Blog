每天都有无数Apple设备运行着依赖于Core Data的应用程序。这使得Core Data成了一个成熟、稳定且非常快速的平台，以供应用程序访问其数据。Core Data本身并不是数据库，它其实是一个拥有诸多功能的框架，而其中一项功能就是把应用程序痛数据库之间的交互过程自动化。

在开发Core Data的应用程序过程中，如果只在控制台的日志中查看Core Data所输出的结果，那么意义并不算太大。我们知不知道这些事情背后究竟发生了什么？Core Data对持久化存储区中的数据到底进行了哪些操作？这些操作是否恰当？为了提供无缝的Core Data体验，系统都生成了哪些SQL查询语句？

有个极其详尽的调试选项可以提供足够的信息，告诉我们这些操作背后所发生的事情，从而令我们知道上述问题的答案。这个调试的选项会把系统自动生成的SQL查询语句打印出来，使开发者深刻认识到Core Data的工作原理。

请按下列步骤修改Grocery Dude，以便开启SQL Debug模式：

1. 点击Product > Scheme > Edit Schem...菜单项。
2. 点击Run Debug，并切换到Argument分页。
3. 点击Arguments Passed On Launch区域中的“+”按钮，以新增参数。
4. 输入新参数**-com.apple.CoreData.SQLDebug 3**，然后点Close按钮。如下图所示。

![设置](http://img.blog.csdn.net/20150924213749700)

现在我们已经开启了第三级（level 3）的SQL Debug模式，然后重新运行应用程序执行数据库相关操作。观察控制台中的日志，会看到，系统自动生成了一些SQL语句，如下图所示。
 
![输出](http://img.blog.csdn.net/20150924213806181)

&#160;

----------

# Appendix

## Related Documentation

[Core Data Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CoreData/index.html#//apple_ref/doc/uid/TP40001075)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974