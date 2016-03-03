#1 block循环引用问题

```objc
//
//  DetailViewController.m
//  test
//
//  Created by admin on 16/3/3.
//  Copyright © 2016年 阳君. All rights reserved.
//

#import "DetailViewController.h"

@interface DetailViewController ()

@end

@implementation DetailViewController

- (void)viewDidLoad
{
    [super viewDidLoad];
    [[NSNotificationCenter defaultCenter] addObserverForName:@"key" object:nil queue:nil usingBlock:^(NSNotification * _Nonnull note) {
        [self notification];
    }];
}

- (void)notification
{
    NSLog(@"%@", NSStringFromSelector(_cmd));
}

- (void)dealloc
{
    NSLog(@"%@", NSStringFromSelector(_cmd));
}

@end
```

当DetailViewController返回上个VC时，发现DetailViewController没有执行dealloc方法释放内存，这就形成了内存泄露。主要由于NSNotificationCenter的block一直持有self，形成了强引用。

#2 ARC模式解决循环引用

ARC模式下使用__block解决循环引用。

```objc
- (void)viewDidLoad
{
    [super viewDidLoad];
    __weak DetailViewController *wSelf = self;
    [[NSNotificationCenter defaultCenter] addObserverForName:@"key" object:nil queue:nil usingBlock:^(NSNotification * _Nonnull note) {
    	[wSelf notification];
    }];
}
```

#3 MRC模式解决循环引用

MRC模式下使用__blcok解决循环引用。

```objc
- (void)viewDidLoad
{
    [super viewDidLoad];
	 __block DetailViewController *wSelf = self;
    [[NSNotificationCenter defaultCenter] addObserverForName:@"key" object:nil queue:nil usingBlock:^(NSNotification * _Nonnull note) {
        [wSelf notification];
    }];
}
```

#4 ReactiveCocoa解决循环引用。

我们可以使用第三方库ReactiveCocoa解决循环引用。

```objc
#import "RACEXTScope.h"

- (void)viewDidLoad
{
	[super viewDidLoad];
	@weakify(self);
	[[NSNotificationCenter defaultCenter] addObserverForName:@"key" object:nil queue:nil usingBlock:^(NSNotification * _Nonnull note) {
		@strongify(self);
		[self notification];
	}];
}

```

&#160;

----------

#Appendix

##Related Documentation

[Blocks Programming Topics](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html)

[ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa#)

[ReactiveCocoa Weak-Strong Dance](http://blog.csdn.net/likendsl/article/details/37764813)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-03 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog