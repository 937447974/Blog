SLComposeServiceViewController可以在共享平台将其他应用的数据共享到我们的应用中，如下图所示。


![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012402.jpg)
Social框架可使我们在分享控件中添加扩展，如Safari的分享功能。这样既可将数据快速分享到我们的应用中。

使用这个框架可使用如下方法：

1. 创建网络Session
2. 获取用户的活动
3. 设置新的post请求
4. 添加附件
5. 发布帖子

# Classes

- NSObject
    - SLComposeSheetConfigurationItem 在发布前配置相关内容
    - SLRequest 封装http请求
- UIViewController
    - SLComposeServiceViewController 社会化分享
    - SLComposeViewController

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Social Framework Reference](https://developer.apple.com/library/ios/documentation/Social/Reference/Social_Framework/index.html)

[SLComposeServiceViewController Class Reference](https://developer.apple.com/library/ios/documentation/Social/Reference/SLComposeServiceViewController_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-24 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog