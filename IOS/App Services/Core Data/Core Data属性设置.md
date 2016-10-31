#1 Data Model Inspector

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
8. Attribute Type：属性类型。

#2 Attribute Type

属性类型，主要分为如下几种。

##2.1 Interger 16、Ingerget 32与 Ingerget 64

对于属性来说，这三种数据类型比较相似的，它们都表示没有小数点的整数，唯一区别就在于能够表示多大或多小的数。

1. Interger 16的取值范围是$-2^{15}$至$2^{15}-1$。
2. Ingerget 32的取值范围是$-2^{31}$至$2^{31}-1$。
3. Ingerget 64的取值范围是$-2^{63}$至$2^{63}-1$。

数值越大，所占的内存就越多。

##2.2 单精度浮点数与双精度浮点数

对于属性来说，单精度浮点数（float）与双精度浮点数（double）这两种数据类型可以看作带小数点的非整数。与float相比，double所包含的二进制位（bit）的个数是它的两倍。

##2.3 小数

在涉及货币或其他十进制运算的场合中，建议把属性的数据设为小数（decimal）。

##2.4 字符串

对于属性来说，字符串这种数据可以存放字符数组（array of character）或普通文本（plain old text）。

##2.5 Boolean

对于属性来说，Boolean这种数据类型可用来存放“是”或“否”这两种值。

##2.6 日期类型

日期（date）这种数据类型就是用来在属性中保存日期和时间得。

##2.7 二进制数据类型

如果要保存照片、音频或其他由“0”、“1”二进制位所组成的连续BLOB,那么就应该把属性的类型设为二进制数据类型（Binary Data）。

##2.8 可变类型

可变（Transformable）数据类型很适合用来把OC对象存放到属性里。

&#160;

----------

#Appendix

##Related Documentation

[Core Data Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CoreData/index.html#//apple_ref/doc/uid/TP40001075)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-26 | 博文完成 |
| 2016-10-31 | 增加Attribute Type章节 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974