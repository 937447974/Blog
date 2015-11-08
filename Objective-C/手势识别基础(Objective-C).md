#UITapGestureRecognizer

手势识别器是当用户在界面的触摸响应，它能帮助我们更好的响应用户的触摸行为，实现更好的交互。如一个按钮的点击，就是一个手势响应。手势的核心是UIGestureRecognizer，它是具体手势识别器的一个抽象基类。它的工作原理如图所示。


它有如下几个子类。

| UIGestureRecognizer | 基类
| :---- | :---- |
| UITapGestureRecognizer | 点击响应 |
| UIPinchGestureRecognizer | 缩放响应 |
| UIRotationGestureRecognizer | 旋转响应 |
| UISwipeGestureRecognizer | 固定方向滑动响应 |
| UIPanGestureRecognizer | 边缘滑动响应 |
| UIScreenEdgePanGestureRecognizer | 滑动响应 | 
| UILongPressGestureRecognizer | 长点击响应 |



当我们在VC中添加一个手势后，用户在界面的触摸，会导致





&#160;

----------

#其他

##参考资料

[Event Handling Guide for iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/GestureRecognizer_basics/GestureRecognizer_basics.html)

[UIGestureRecognizer Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIGestureRecognizer_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-7 | 苹果手势识别基础总结 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j