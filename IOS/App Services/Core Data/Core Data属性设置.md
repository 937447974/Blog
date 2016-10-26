当我们使用Xcode建立设置表中字段时，可看见有如下界面。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016102602.png)

在图片的最右则就是Data Model Inspector。其中的选项作用如下：

1. Transient：如果在Properties中勾选了这一项，那么该特性就不会写入持久化存储区。
2. Optional：表示不一定要有值。所有的特性在刚被创建出来的时候就是optional特性。
3. Indexed：系统会优化Indexed特性以提升搜索效率，但代价是要在底层的持久化存储区中占用额外的控件。
4. Validation：阻止不合理的数据进入持久化存储区。
5. Default：默认值。
6. Index in Spotlight：这个选项不会影响iOS应用程序，它的用途是把基于CoreData的应用程序通Spotlight集成起来。
7. Store in External Record File：启用了该选项之后，系统会把持久化存储区里的数据复制成XML格式，并保存在存储区之外。

&#160;

----------

#Appendix

##Related Documentation

[Core Data Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CoreData/index.html#//apple_ref/doc/uid/TP40001075)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-26 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974