
UIWindow是一种特殊的UIView，通常在一个app中只会有一个UIWindow。

iOS程序启动完毕后，创建的第一个视图控件就是UIWindow，接着创建控制器的view，最后将控制器的view添加到UIWindow上，于是控制器的view就显示在屏幕上了。

一个iOS程序之所以能显示到屏幕上，完全是因为它有UIWindow。也就说，没有UIWindow，就看不见任何UI界面。

直接将控制器的view添加到UIWindow中，并不理会它对应的控制器

```objc
[self.window  addsubview:vc.view];
```

设置uiwindow的根控制器，自动将rootviewcontroller的view添加到window中，负责管理rootviewcontroller的生命周期

```objc
[self.window.rootviewcontroller = vc];
```

主窗口和次窗口

```objc
[self.window makekeyandvisible]; // 让窗口成为主窗口，并且显示出来。有这个方法，才能把信息显示到屏幕上。

[self.window makekeywindow]; // 让uiwindow成为主窗口，但不显示。
```

获取UIwindow

1. [UIApplication sharedApplication].windows  在本应用中打开的UIWindow列表，这样就可以接触应用中的任何一个UIView对象(平时输入文字弹出的键盘，就处在一个新的UIWindow中)
2. UIApplication sharedApplication].keyWindow（获取应用程序的主窗口）用来接收键盘以及非触摸类的消息事件的UIWindow，而且程序中每个时刻只能有一个UIWindow是keyWindow。
3. view.window获得某个UIView所在的UIWindow

> 如果某个UIWindow内部的文本框不能输入文字，可能是因为这个UIWindow不是keyWindow.

&#160;

----------

#其他

##参考资料

 [iOS开发UI篇—UIWindow简单介绍 - 文顶顶](http://www.tuicool.com/articles/UNVJjaN)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-11 | UIWindow简单介绍 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j