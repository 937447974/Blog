#1 需求

在开发过程中，我们会使用NSLog打印一些日志，如果在NSArray中有中文字符时，如下。

```objc
NSArray *array = [NSArray arrayWithObjects:@"阳君", nil];
NSDictionary *dict = [NSDictionary dictionaryWithObjectsAndKeys:array, @"name", @"937447974", @"qq", nil];
array = [NSArray arrayWithObjects:@"阳君", dict, nil];
dict = [NSDictionary dictionaryWithObjectsAndKeys:array, @"name", @"937447974", @"qq", nil];
NSLog(@"%@", dict);
```

打印出来会显示Unicode码。这样很不便于开发过程的打印。




&#160;

----------

#其他

##源代码

https://github.com/937447974/Objective-C

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-16 | NSLog打印优化 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog