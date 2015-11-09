[手势识别基础(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49719503)

----------

在上一篇博文，我们已经讲解了关于手势识别的基础知识。这篇博文我们会实现下列需求。

1. 使用UIPinchGestureRecognizer缩放View；
2. 使用UIRotationGestureRecognizer旋转View；
3. 使用UISwipeGestureRecognizer移动View；
4. 同时支持缩放、旋转和移动View。

#项目准备

##创建项目

你可以使用上一篇博文的项目，也可以使用新的项目，这里没有任何特殊要求，只是我们只使用gestureView。

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