#UITapGestureRecognizer

手势识别器是当用户在界面的触摸响应，它能帮助我们更好的响应用户的触摸行为，实现更好的交互。如一个按钮的点击，就是一个手势响应。手势的核心是UIGestureRecognizer，它是具体手势识别器的一个抽象基类。它的工作原理如图所示。

![UITapGestureRecognizer-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110801.png)

从图中我们可以看出：

1. 当用户在View界面触摸时，通知手势识别器；
2. 手势识别器接受到手势通知后，通过我们预先添加的响应链通知类myViewController的方法respondToGesture；
3. myViewController的respondToGesture方法体接受到手势通知后执行我们相应执行的业务。

在UITapGestureRecognizer中有如下几个子类，也是我们常用的手势识别器。

| UIGestureRecognizer | 基类
| :---- | :---- |
| UITapGestureRecognizer | 点击响应 |
| UIPinchGestureRecognizer | 缩放响应 |
| UIRotationGestureRecognizer | 旋转响应 |
| UISwipeGestureRecognizer | 固定方向滑动响应 |
| UIPanGestureRecognizer | 边缘滑动响应 |
| UIScreenEdgePanGestureRecognizer | 滑动响应 | 
| UILongPressGestureRecognizer | 长点击响应 |

打开UITapGestureRecognizer类，你会看到如下的通用方法和属性，这里我们只介绍其中比较重要的。

- `- (instancetype)initWithTarget:(nullable id)target action:(nullable SEL)action`：初始化时，并添加响应的VC和方法体。如

```objective-c
UITapGestureRecognizer *tapGR = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tapAction:)];
```

- `- (void)addTarget:(id)target action:(SEL)action; `：初始化后添加，可以同时添加多个响应的target和action。
- `- (void)removeTarget:(nullable id)target action:(nullable SEL)action;` ： 删除响应的target和action。

```objective-c
[gr addTarget:self action:@selector(tapAction:)];
[gr removeTarget:self action:@selector(tapAction:)];
```

- `@property(nonatomic,readonly) UIGestureRecognizerState state` : 手势的枚举状态，可以在回调方法中判断当前手势的状态。

```objective-c
typedef NS_ENUM(NSInteger, UIGestureRecognizerState) {
    UIGestureRecognizerStatePossible,   // 准备手势响应
    UIGestureRecognizerStateBegan,      // 开始手势响应
    UIGestureRecognizerStateChanged,    // 手势响应持续中，如旋转手势的连续旋转
    UIGestureRecognizerStateEnded,      // 手势响应结束
    UIGestureRecognizerStateCancelled,  // 手势响应取消
    UIGestureRecognizerStateFailed,     // 手势响应丢弃，如旋转手势识别器接受到一个缩放手势，则会不响应则个手势，将其通过手势链向下抛。
    UIGestureRecognizerStateRecognized = UIGestureRecognizerStateEnded // 手势已响应
};
```

它们的运作方式如下图所示

![UITapGestureRecognizer-2](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110802.png)

> 这里运用到了COR设计模式，在IOS的界面，苹果公司运用了大量的COR模式。如果你不知道什么是COR，可以阅读我的另一篇博文《[23设计模式之责任链模式 (COR)](http://blog.csdn.net/y550918116j/article/details/48596903)》
 
- `@property(nullable,nonatomic,weak) id <UIGestureRecognizerDelegate> delegate`：手势的代理，你可以通过它监听手势的每一个细微的改变。
- `@property(nonatomic, getter=isEnabled) BOOL enabled`: 开启还是关闭手势响应，默认开启YES；
- `@property(nullable, nonatomic,readonly) UIView *view`：手势控制的View,给他View添加和删除的方法如下:

```objective-c
[self.view addGestureRecognizer:gr];    // 添加
[self.view removeGestureRecognizer:gr]; // 删除
```

- `- (void)requireGestureRecognizerToFail:(UIGestureRecognizer *)otherGestureRecognizer`：当otherGestureRecognizer的手势响应跳过时才执行当前手势的响应。

// individual UIGestureRecognizer subclasses may provide subclass-specific location information. see individual subclasses for details
- (CGPoint)locationInView:(nullable UIView*)view;                                // a generic single-point location for the gesture. usually the centroid of the touches involved

- (NSUInteger)numberOfTouches;                                          // number of touches involved for which locations can be queried
- (CGPoint)locationOfTouch:(NSUInteger)touchIndex inView:(nullable UIView*)view; // the location of a particular touch

@end




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