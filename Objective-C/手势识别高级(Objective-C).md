[手势识别基础(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49719503)

----------

在上一篇博文，我们已经讲解了关于手势识别支持缩放、旋转和移动View的相关知识。这篇博文我们会实现在app中常见的功能：手势解锁。

#前期准备

在前面我们讲解了手势识别器，今天运用到的功能不需要手势识别器来实现，当然你也可以使用手势识别器的UISwipeGestureRecognizer来实现。

在所有UIViewController子类中都有如下几个方法：

- `- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(nullable UIEvent *)event`：手指开始触摸屏幕；
- `- (void)touchesMoved:(NSSet<UITouch *> *)touches withEvent:(nullable UIEvent *)event`：手指在屏幕上触摸；
- `- (void)touchesEnded:(NSSet<UITouch *> *)touches withEvent:(nullable UIEvent *)event`：手指离开屏幕。

这几个方法和viewDidLoad一样，你都可以继承实现。今天就是使用这几个方法去做手势解锁的功能。

#项目准备

##创建项目

你可以使用上一篇博文的项目，也可以使用新的项目，这里没有任何特殊要求。

![缩放、旋转和移动-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110910.jpg)


创建后运行如下；

![缩放、旋转和移动-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110907.jpg)

图中用到的资源文件如下

![缩放、旋转和移动-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110907.jpg)

为了提高开发效率，我们使用拉控件的方式创建手势识别器。

##核心类



![缩放、旋转和移动-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110813.jpg)

&#160;

----------

#其他

##参考资料

[Event Handling Guide for iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/GestureRecognizer_basics/GestureRecognizer_basics.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-8 | 使用苹果手势识别器实现手势解锁功能 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j