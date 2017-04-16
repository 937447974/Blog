# 1 NSLog

在开发过程中，我们会使用NSLog打印一些日志，如果在NSArray或NSDictionary中有中文字符时，如下。

```objc
NSArray *array = [NSArray arrayWithObjects:@"阳君", nil];
NSDictionary *dict = [NSDictionary dictionaryWithObjectsAndKeys:array, @"name", @"937447974", @"qq", nil];
array = [NSArray arrayWithObjects:@"阳君", dict, nil];
dict = [NSDictionary dictionaryWithObjectsAndKeys:array, @"name", @"937447974", @"qq", nil];
NSLog(@"%@", dict);
```

打印出来会显示Unicode码。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111701.png)

如图这样很不利于开发过程的调试。如果你说这样很好，你看的懂，那你牛逼！

# 2 NSLog打印优化

通过分析打印的实质是调用了NSArray或NSDictionary对象的`- (NSString *)descriptionWithLocale:(id)locale`方法。我们只需重写这个方法即可。

这里使用了类NSLog+Extension。+号可以告诉大家，这是一个扩展类。

NSLog+Extension.h代码如下

```objc
//
//  NSLog+Extension.h
//  NSLog
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/17.
//  Copyright © 2015年 阳君. All rights reserved.
//

# import <Foundation/Foundation.h>

/** 数组NSLog打印扩展*/
@interface NSArray (NSLogExtension)

@end

/** 字典NSLog打印扩展*/
@interface NSDictionary (NSLogExtension)

@end

/** NSSet打印扩展*/
@interface NSSet (NSLogExtension)

@end
```

NSLog+Extension.m代码如下

```objc
//
//  NSLog+Extension.m
//  NSLog
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/11/17.
//  Copyright © 2015年 阳君. All rights reserved.
//

# import "NSLog+Extension.h"

# if 1

#pragma mark -  NSLog打印辅助方法
id logExtension(id obj) {
    id tempObj = obj;
    // 遇到NSArray、NSSet或NSDictionary的子类，内容后移\t
    if ([obj isKindOfClass:[NSDictionary class]] || [obj isKindOfClass:[NSArray class]] || [obj isKindOfClass:[NSSet class]]) {
        NSString *str = [NSString stringWithFormat:@"%@", obj];
        str = [str stringByReplacingOccurrencesOfString:@"\n" withString:@"\n\t"];
        tempObj = str;
    } else if ([obj isKindOfClass:[NSString class]]) { // NSString类型数据加双引号
        tempObj = [NSString stringWithFormat:@"\"%@\"", obj];
    }
    return tempObj;
}


#pragma mark - 数组NSLog打印扩展
@implementation NSArray (NSLogExtension)

#pragma mark 数组打印
- (NSString *)descriptionWithLocale:(id)locale {
    NSMutableString *str = [NSMutableString stringWithString:@"(\n"];
    // 遍历数组的所有元素
    for (id obj in self) {
        [str appendFormat:@"\t%@,\n", logExtension(obj)];
    }
    [str appendString:@")"];
    return str;
}

@end


#pragma mark - 字典NSLog打印扩展
@implementation NSDictionary (NSLogExtension)

#pragma mark 字典打印
- (NSString *)descriptionWithLocale:(id)locale {
    __block NSMutableString *str = [NSMutableString stringWithString:@"{\n"];
    // 遍历字典的所有键值对
    [self enumerateKeysAndObjectsUsingBlock:^(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {
        [str appendFormat:@"\t%@ = %@,\n", key, logExtension(obj)];
    }];
    [str appendString:@"}"];
    // 删掉最后一个,
    NSRange range = [str rangeOfString:@"," options:NSBackwardsSearch];
    // 保护机制找到才删除
    if (range.location > 0 && range.location < str.length) {
        [str deleteCharactersInRange:range];
    }
    return str;
}

@end


#pragma mark - NSSet NSLog打印扩展
@implementation NSSet (NSLogExtension)

#pragma mark NSSet打印
- (NSString *)descriptionWithLocale:(id)locale {
    NSMutableString *str = [NSMutableString stringWithString:@"{(\n"];
    for (id value in self) {
        NSLog(@"%@", value);
        [str appendFormat:@"\t%@,\n", logExtension(value)];
    }
    [str appendString:@")}"];
    // 删掉最后一个,
    NSRange range = [str rangeOfString:@"," options:NSBackwardsSearch];
    // 保护机制找到才删除
    if (range.location > 0 && range.location < str.length) {
        [str deleteCharactersInRange:range];
    }
    return str;
}

@end

# endif
```

我们使用字符串拼接的方式返回要打印的字符串。这里使用了函数logExtension，主要是因为在数组中包含字典或数组时，要让显示数据后移'\t'。

# 3 测试

只需引入文件，无须在类中引入头文件。再次运行前面的测试代码，打印输出

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111702.png)

可以发现两个的打印结果格式没有太大差异，但是优化后的打印看着就是爽。

# 4 小结

本博文讲解了工作中的常用的NSLog打印优化，其实还有一种就是将数据转换为NSString再查看。但是没有格式，数据庞大时，不利于查看，如果你有兴趣可以查看的的博文《[JSON解析](http://blog.csdn.net/y550918116j/article/details/49002701)》。

&#160;

----------

# 其他

## 源代码

https://github.com/937447974/Objective-C

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-16 | NSLog打印优化 |
| 2015-11-19 | 修改源代码，字典遍历不使用并发，防止线程错误 |
| 2015-12-29 | 增加NSSet打印扩展，增加对NSString的支持 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog