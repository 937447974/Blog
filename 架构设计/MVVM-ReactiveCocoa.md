在上一篇博文，我们讲解了[MVVM框架](https://github.com/937447974/Blog/blob/master/架构设计/MVVM框架.md)的相关知识。在这篇博文将使用ReactiveCocoa第三方库实现MVVM架构。


#1 引入ReactiveCocoa库

使用CocoaPods引入第三分库ReactiveCocoa。Podfile内相关内容如下：

```pod
platform :ios, '9.0'

pod 'ReactiveCocoa', '~> 2.3.1'
```

#2 界面设计

在这里我们模拟登录功能。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016030801.png)

YJViewController类代码如下。

```objc
//
//  YJViewController.m
//  MVVM-ReactiveCocoa
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by admin on 16/3/7.
//  Copyright © 2016年 阳君. All rights reserved.
//

#import "YJViewController.h"
#import <ReactiveCocoa/ReactiveCocoa.h>
#import "RACEXTScope.h"
#import "YJViewModel.h"

@interface YJViewController ()

@property (weak, nonatomic) IBOutlet UITextField *userNameTextField; ///< 用户名
@property (weak, nonatomic) IBOutlet UITextField *passwordTextField; ///< 密码
@property (weak, nonatomic) IBOutlet UIButton *loginButton;          ///< 登录按钮

@property (nonatomic, strong) YJViewModel *viewModel; ///< ViewModel对象

@end

@implementation YJViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
}

@end
```

#3 MVVM实现

##3.1 ViewModel

我们实现一个简单的ViewModel层。

YJViewModel.h

```objc
//
//  YJViewModel.h
//  MVVM-ReactiveCocoa
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by admin on 16/3/7.
//  Copyright © 2016年 阳君. All rights reserved.
//

#import <Foundation/Foundation.h>

/** ViewModel完全把Model和View进行了分离，主要的程序逻辑在ViewModel里实现*/
@interface YJViewModel : NSObject

@property (nonatomic, copy) NSString *userName; ///< 用户名
@property (nonatomic, copy) NSString *password; ///< 密码

@property (nonatomic, copy) NSString *title; ///< 标题

/**
 *  初始化相关数据
 *
 *  @return void
 */
- (void)initData;

@end
```

YJViewModel.m

```objc
//
//  YJViewModel.m
//  MVVM-ReactiveCocoa
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by admin on 16/3/7.
//  Copyright © 2016年 阳君. All rights reserved.
//

#import "YJViewModel.h"

@implementation YJViewModel

- (void)initData
{
    NSLog(@"用户名:%@; 密码:%@", self.userName, self.password);
    self.title = @"阳君：937447974";
}

@end
```

##3.2 引入ReactiveCocoa实现MVVM

下面改造YJViewController.m完成和ViewModel层的绑定。

这里实现的需求如下:

1. 时刻监听用户的输入，当用户输入的用户名和密码大于三位时才能点击登录按钮。
2. 用户输入错误时输入框显示红色，正确时显示绿色。
3. 用户输入错误登录按钮不可点，输入正确登录按钮可点。
4. 用户点击登录按钮1s内重复点击无效。

核心代码实现如下:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    // 校验用户输入信息
    RACSignal *validUserNameSignal = [self.userNameTextField.rac_textSignal map:^id(NSString *value) {
        return @(value.length > 3);
    }];
    RACSignal *validPasswordSignal = [self.passwordTextField.rac_textSignal map:^id(NSString *value) {
        return @(value.length > 3);
    }];
    
    // 根据用户输入信息修改输入框背景色
    RAC(self.userNameTextField, backgroundColor) = [validUserNameSignal map:^id(NSNumber *value) {
        return value.boolValue ? [UIColor greenColor] : [UIColor redColor];
    }];
    RAC(self.passwordTextField, backgroundColor) = [validPasswordSignal map:^id(NSNumber *value) {
        return value.boolValue ? [UIColor greenColor] : [UIColor redColor];
    }];
    
    // 按钮能否点击
    RAC(self.loginButton, enabled) = [RACSignal combineLatest:@[validUserNameSignal, validPasswordSignal] reduce:^id(NSNumber *usernameValid, NSNumber *passwordValid){
        return @([usernameValid boolValue] && [passwordValid boolValue]);
    }];
    
    @weakify(self);
    // 1s内禁止重复点击
    [[[[self.loginButton rac_signalForControlEvents:UIControlEventTouchUpInside] map:^id(id value) {
        @strongify(self);
        self.loginButton.enabled = NO;
        [self.viewModel initData];
        return @(true);
    }] throttle:1] subscribeNext:^(id x) {
        @strongify(self);
        self.loginButton.enabled = YES;
    }];
    
    // viewModel绑定
    self.viewModel = [[YJViewModel alloc] init];
    RAC(self.viewModel, userName) = [self.userNameTextField.rac_textSignal map:^id(id value) {
        return value;
    }];
    RAC(self.viewModel, password) = [self.passwordTextField.rac_textSignal map:^id(id value) {
        return  value;
    }];
    RAC(self, title) = RACObserve(self.viewModel, title);
    
}
```

点击运行即可看见相应效果。

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Framework)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-08 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog