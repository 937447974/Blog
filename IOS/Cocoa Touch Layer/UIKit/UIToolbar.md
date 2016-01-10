我们在开发过程中会发现UINavigationBar并不能放太多的按钮，如右边超过2个就会很难看，此时有更多的按钮我们该怎么办呢？苹果考虑这种情况提供了UIToolbar。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016011001.jpg)

UIToolbar的位置如图所示，在屏幕的最下方，这样在一个VC中即可提供更多的按钮给用户使用。

我们还可以获取修改整个系统的UIToolbar，只需使用`UIToolbar.appearance()`拿到系统的UIToolbar，然后修改其中相关参数即可。

如果要修改当前UIToolbar

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[UIKit Framework Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

[UINavigationController Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UINavigationController_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-07 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog