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

在StoryBoard控件处你可以看见这些控件。

![UITapGestureRecognizer-6](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110806.png)

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
- `- (NSUInteger)numberOfTouches`: 用户触摸的手指数。 

#项目准备

##创建项目

创建一个单一项目GestureRecognizer

![UITapGestureRecognizer-3](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110803.png)

这里我使用了两个View，一个是界面直接拉去手势识别器的测试，一个是通过代码添加手势识别器的测试。当然你不一定要跟我一样，在这里我还使用了tab控件，是为了讲解进阶和高级做准备的。两个View和最底层的View最好使用不同的颜色做区分，有助于提示你，你正在那个界面做手势操作。

##核心类

我们使用的核心类是BaseVC。下面是核心代码。

```objective-c
//
//  BaseVC.m
//  GestureRecognizer
//
//  Created by yangjun on 15/11/8.
//  Copyright © 2015年 六月. All rights reserved.
//

#import "BaseVC.h"

@interface BaseVC ()

@property (weak, nonatomic) IBOutlet UIView *gestureView;     ///< 手势界面(视图添加)
@property (weak, nonatomic) IBOutlet UIView *gestureCodeView; ///< 手势界面(代码添加)

@end

@implementation BaseVC

- (void)viewDidLoad {
    [super viewDidLoad];
    
    BOOL codeHidden = NO; // YES视图测试，NO代码测试
    self.gestureView.hidden = !codeHidden;
    self.gestureCodeView.hidden = codeHidden;
    self.gestureCodeView.userInteractionEnabled = YES; // 开启手势响应
    
}

@end
```

在BaseVC中，我们通过View的hidden控制当前是做试图添加的手势测试还是代码添加的手势测试。将BaseVC和试图连接起来后，运行项目。

![UITapGestureRecognizer-4](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110804.png)

#UITapGestureRecognizer

UITapGestureRecognizer是单点击的手势识别器，你也可以理解为短点击手势识别器，如UIButton的点击响应。

在UITapGestureRecognizer中有如下属性。

- `@property (nonatomic) NSUInteger  numberOfTapsRequired`: 用户点击的次数，默认为1；
- `@property (nonatomic) NSUInteger  numberOfTouchesRequired __TVOS_PROHIBITED`:用户使用的手指数，默认1；

使用视图添加手势后，你可以通过设置栏设置相关属性。

![UITapGestureRecognizer-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110805.png)

在代码中添加方法体，然后连接运行即可看见效果。

```objective-c
#pragma mark UITapGestureRecognizer 点击
- (IBAction)tapAction:(UITapGestureRecognizer *)sender {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    NSLog(@"点击数:%lu; 手指数:%lu", (unsigned long)sender.numberOfTapsRequired, (unsigned long)sender.numberOfTouchesRequired);
    CGPoint location = [sender locationInView:self.view];
    NSLog(@"点击的位置:%@", NSStringFromCGPoint(location));
}
```

当然你也可以直接在viewDidLoad方法中代码添加UITapGestureRecognizer。

```objective-c
//UITapGestureRecognizer 短点击
UITapGestureRecognizer *tapGR = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tapAction:)];// 初始化指定回调target和方法action
tapGR.numberOfTapsRequired = 1; // 手指连续点击的次数，默认1
tapGR.numberOfTouchesRequired = 1; // 有几个手指点击，默认1
[self.gestureCodeView addGestureRecognizer:tapGR];
```

>你只需修改codeHidden的值即可切换storyboard测试还是代码测试。为节约篇幅，以后会直接显示响应的方法体和代码添加手势的相关代码。

#UILongPressGestureRecognizer

与短点击相对应的还有长点击手势UILongPressGestureRecognizer。

在UILongPressGestureRecognizer中有如下属性。

- `@property (nonatomic) NSUInteger numberOfTapsRequired`: 长点击响应前点击次数,默认0；
- `@property (nonatomic) NSUInteger numberOfTouchesRequired __TVOS_PROHIBITED`: 用户触摸的手指数，默认1；
- `@property (nonatomic) CFTimeInterval minimumPressDuration`: 长按最低时间，默认0.5秒；
- `@property (nonatomic) CGFloat allowableMovement`: 手指长按期间可移动的区域，默认10像素。

使用视图添加手势后，你可以通过设置栏设置相关属性。



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