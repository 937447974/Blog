NSBundleResourceRequest是用于按需加载资源的下载控制。按需加载资源是由App Store托管的内容，它和下载的app bundle是分开的。app请求一系列按需加载资源，而下载和存储资源是由操作系统来管理。这些资源可以是除可执行代码外，bundle支持的任何类型。

支持的类型

| Type | Asset catalog | File |
| ---- | :--: | :--: |
| Data file | ✓ | ✓ |
| Image | ✓ | ✓ |
| OpenGL shader | – | ✓ |
| SpriteKit particle | – | ✓ |
| SpriteKit scene | – | ✓ |
| SpriteKit texture atlas | ✓ | ✓ |
| Apple TV Image Stack | ✓ | ✓ |

按需加载资源的好处有如下几点：

- 缩小app大小；
- 懒加载资源文件；
- 很少使用的资源放在远端；
- 内购的资源放在远端。

按需加载资源的生命周期如下所示：

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016031601.png)

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[NSBundleResourceRequest Class Reference](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSBundleResourceRequest_Class/index.html)

[On-Demand Resources Essentials](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html)

[NSBundle Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-15 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog