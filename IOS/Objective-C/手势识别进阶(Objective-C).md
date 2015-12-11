[手势识别基础(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49719503)

[手势识别进阶(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49720451)

[手势识别进阶(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49743123)

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

在这里我们使用的核心类为AdvancedVC。

```objective-c
//
//  AdvancedVC.m
//  GestureRecognizer
//
//  Created by yangjun on 15/11/8.
//  Copyright © 2015年 六月. All rights reserved.
//

#import "AdvancedVC.h"

@interface AdvancedVC ()
{
    CGFloat _scale;    // 缩放比例
    CGFloat _rotation; // 旋转比例
    CGPoint _translatedPoint; // 移动的位置
}

@property (weak, nonatomic) IBOutlet UIView *gestureView;  ///< 界面

@end

@implementation AdvancedVC

- (void)viewDidLoad {
    [super viewDidLoad];
    _scale = 1.0;
    _rotation = 0;
    _translatedPoint = CGPointMake(0, 0);
}

@end
```

我已经为大家设置了几个私有变量_scale、_rotation和_translatedPoint，并设置了相应默认值，这将为以后的开发做准备。

#缩放

缩放gestureView使用到了UIPinchGestureRecognizer，其核心方法如下。

```objective-c
#pragma mark UIPinchGestureRecognizer 缩放
- (IBAction)pinchAction:(UIPinchGestureRecognizer *)sender {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    // 放大+；缩小-
    NSLog(@"缩放比例:%f; 缩放速度:%f", sender.scale, sender.velocity);
    CGFloat scale = _scale * sender.scale; // 在上次的缩放比例上进行缩放 
    // 改变transform 缩放
    self.gestureView.transform = CGAffineTransformMakeScale(scale, scale);
    // 结束时记录缩放比例
    if (sender.state == UIGestureRecognizerStateEnded) {
        _scale = scale;
    }
}
```

这里需要注意的地方有：

1. 这里我们使用了CGAffineTransformMakeScale，它能帮我们计算需要CGAffineTransform，只需传入相应x和y的缩放比例；
2. 我们的UIPinchGestureRecognizer控制的View不是gestureView，而是self.view。这是为了在gestureView的外部也可通过手势缩放gestureView；
3. 手势结束时要记录缩放的比例。

#旋转

旋转gestureView使用了UIRotationGestureRecognizer，其核心方法如下：

```objective-c
#pragma mark UIRotationGestureRecognizer 旋转
- (IBAction)rotationAction:(UIRotationGestureRecognizer *)sender {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    // 顺时针+；逆时针-
    NSLog(@"旋转比例:%f; 旋转速度:%f", sender.rotation, sender.velocity);
    CGFloat rotation = _rotation + sender.rotation; // 在上次的旋转比例上进行旋转
    // 角度缩到-2M_PI到2M_PI，优化旋转所需内存。M_PI代表顺时针旋转180度
    while (rotation < -2 * M_PI && 2 * M_PI < rotation) {
        if (rotation > 0) {
            rotation -= 2 * M_PI;
        } else {
            rotation += 2 * M_PI;
        }
    }
    self.gestureView.transform = CGAffineTransformMakeRotation(rotation); // 旋转
    // 结束时记录旋转比例
    if (sender.state == UIGestureRecognizerStateEnded) {
        _rotation = rotation;
    }
}
```

这里需要注意的地方有：

1. 使用CGAffineTransformMakeRotation修改self.gestureView.transform，只需传入偏转数字；
2. rotation是以`M_PI`做比例，一个`M_PI`代表顺时针180度，这里我们使用while将其控制在-360°~360°，优化了内存；
3. 结束时记录旋转度；

#移动

移动gestureView使用了UIPanGestureRecognizer，核心代码如下。

```objective-c
#pragma mark UIPanGestureRecognizer 平滑
- (IBAction)panAction:(UIPanGestureRecognizer *)sender {
    NSLog(@"%@", NSStringFromSelector(_cmd));
    CGPoint translatedPoint = [sender translationInView:sender.view]; // 移动的位移
    NSLog(@"移动位移:%@", NSStringFromCGPoint(translatedPoint));
    translatedPoint.x += _translatedPoint.x;
    translatedPoint.y += _translatedPoint.y;
    // 改变transform
    self.gestureView.transform = CGAffineTransformMakeTranslation(translatedPoint.x, translatedPoint.y);
    if (sender.state == UIGestureRecognizerStateEnded) {
        _translatedPoint = translatedPoint;
        [sender setTranslation:CGPointMake(0, 0) inView:self.view];
    }
}
```

这里需要注意的地方有：

1. 使用CGAffineTransformMakeTranslation修改self.gestureView.transform，只需传入移动x和y的相对位移；
2. 手势结束时记录移动的相对位移；

#缩放、旋转和移动三合一

如果你觉得只需要上面的代码就可完成三合一，那你就想的有点简单了。

通过我们的测试发现，我们无法让三个手势相互影响。比如我旋转了一定角度，然后放大gestureView，发现gestureView只实现了放大效果，而没有执行旋转效果。

通过分析发现我们设置是通过self.gestureView.transform设置的，它会覆盖上一次的transform，无法起到叠加效果。

查看CGAffineTransform源文件，发现如下三个方法体。

- CG_EXTERN CGAffineTransform CGAffineTransformTranslate(CGAffineTransform t,
  CGFloat tx, CGFloat ty)；
- CG_EXTERN CGAffineTransform CGAffineTransformScale(CGAffineTransform t,
  CGFloat sx, CGFloat sy);
- CG_EXTERN CGAffineTransform CGAffineTransformRotate(CGAffineTransform t,
  CGFloat angle);

它们能帮助我们对transform产生叠加效果。

现在封装一个公用方法，传入scale缩放比例、rotation旋转比例和translatedPoint移动的位置其叠加效果。

```objective-c
#pragma mark 根据scale缩放比例、rotation旋转比例、translatedPoint移动的位置改变视图
- (void)setTransform:(CGFloat)scale rotation:(CGFloat)rotation translatedPoint:(CGPoint)translatedPoint {
    // 平移的速度会受到缩放比例和旋转比例的影响
    CGAffineTransform transform = CGAffineTransformMakeScale(scale, scale); // 缩放
    transform = CGAffineTransformRotate(transform, rotation); // 旋转
    transform = CGAffineTransformTranslate(transform, translatedPoint.x, translatedPoint.y); // 平移
    // 改变transform
    self.gestureView.transform = transform;
}
```

这个叠加顺序不能做修改，不然达不到预期的效果，初步怀疑是苹果的bug。

然后修改上面三个方法中的一行代码即可。

缩放`- pinchAction:`

```objective-c
self.gestureView.transform = CGAffineTransformMakeScale(scale, scale);
改成
[self setTransform:scale rotation:_rotation translatedPoint:_translatedPoint];
```

旋转`- rotationAction:`

```objective-c
self.gestureView.transform = CGAffineTransformMakeRotation(rotation);
改成
[self setTransform:_scale rotation:rotation translatedPoint:_translatedPoint]; 
```

移动`- panAction:`

```objective-c
self.gestureView.transform = CGAffineTransformMakeTranslation(translatedPoint.x, translatedPoint.y);
改成
[self setTransform:_scale rotation:_rotation translatedPoint:translatedPoint];
```

运行项目在界面做相应手势操作，你就能看见如下效果图。

![缩放、旋转和移动-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015110813.jpg)

&#160;

----------

#其他

##参考资料

[Event Handling Guide for iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/GestureRecognizer_basics/GestureRecognizer_basics.html)

[UIGestureRecognizer Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIGestureRecognizer_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-08 | 使用苹果手势识别器实现很炫的旋转、缩放和移动为一体的动效 |
| 2015-11-09 | 添加相应链接[手势识别基础(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49719503)、[手势识别进阶(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49720451)和[手势识别进阶(Objective-C)](http://blog.csdn.net/y550918116j/article/details/49743123)|

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j