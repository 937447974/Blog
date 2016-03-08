在上一篇博文，我们讲解了[MVVM框架](https://github.com/937447974/Blog/blob/master/架构设计/MVVM框架.md)的相关知识。在这篇博文将使用ReactiveCocoa第三方库实现MVVM架构。


#1 引入ReactiveCocoa库

使用CocoaPods引入第三分库ReactiveCocoa。Podfile内相关内容如下：

```pod
platform :ios, '9.0'

pod 'ReactiveCocoa', '~> 2.3.1'
```

#2 界面设计

在这里我们模拟登录功能。




![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Contacts Framework Reference](https://developer.apple.com/library/ios/documentation/Contacts/Reference/Contacts_Framework/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-13 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog