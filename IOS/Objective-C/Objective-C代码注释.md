类注释

```objc
@file: 使用这个标签来指出你正在记录一个文件（header 文件或不是）。如果你将使用Doxygen来输出文档，那么你最好在这个标签后面紧接着写上文件名字。它是一个top level 标签。

@header: 跟上面的类似，但是是在 HeaderDoc中使用。当你不使用 Doxygen时，不要使用上面的标签。

@author：用它来写下这个文件的创建者信息

@copyright: 添加版权信息

@version: 用它来写下这个文件的当前版本。如果在工程生命周期中版本信息有影响时这会很重要。

再一次的，我只给出最常用的标签。自己查看说明文档了解更多标签信息。

@class: 用它来指定一个class的注释文档块的开头。它是一个top level标签，在它后面应该给出class名字。

@interface: 同上

@protocol: 同上两个一样，只是针对protocols

@superclass: 当前class的superclass

@classdesign: 用这个标签来指出你为当前class使用的任何特殊设计模式（例如，你可以提到这个class是不是单例模式或者类似其它的模式）。

@coclass: 与当前class合作的另外一个class的名字。

@helps: 当前class帮助的class的名字。

@helper: 帮助当前class的class名字。
```

属性注释

```objc
/** 描述*/
@property (nonatomic, ) ;
// 或
/** */
@property (nonatomic, ) ; ///< 描述
```

方法注释

```objc
@brief : 使用它来写一段你正在文档化的method, PRoperty, class, file, struct, 或enum的短描述信息。
@discusstion: 用它来写一段详尽的描述。如果需要你可以添加换行。
@param:通过它你可以描述一个 method 或 function的参数信息。你可以使用多个这种标签。
@return: 用它来制定一个 method 或 function的返回值。
@see: 用它来指明其他相关的 method 或 function。你可以使用多个这种标签。
@sa:同上
@code ： 使用这个标签，你可以在文档当中嵌入代码段。当在Help Inspector当中查看文档时，代码通过在一个特别的盒子中用一种不同的字体来展示。始终记住在写的代码结尾处使用@endcode标签。
@remark : 在写文档时，用它来强调任何关于代码的特殊之处。
```

&#160;

----------

#Appendix

##Related Documentation

[iOS使用appledoc 生成技术API文档详解](http://www.jianshu.com/p/65f1afdb9445)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-08-24 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974