以Xcode8举例，Xcode8支持ios8以下真机测试方法如下：

1. Xcode 7显示包内容-Contents-Developer-Platforms-iPhoneOS.platform-DeviceSupport 把里边 6.0 6.1 7.0 7.1 的文件夹粘贴到xcode8 对应的文件夹内  
2. 应用程序-Xcode 8显示包内容-Contents-Developer-Platforms-iPhoneOS.platform-Developer-SDKs-iPhoneOS.sdk-SDKSettings.plist 文件下DefaultProperties - DEPLOYMENT_TARGET_SUGGESTED_VALUES该数组中添加 6.0 6.1 7.0 7.1 对应的测试版本

&#160;

----------

#Appendix

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-12-22 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974