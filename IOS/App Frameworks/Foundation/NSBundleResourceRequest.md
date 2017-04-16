1. [Initializing a Resource Request](#1)
2. [Accessing the Configuration](#2)
3. [Requesting Resources](#3)
4. [Setting the Download Priority](#4)
5. [Tracking Progress](#5)

----

NSBundleResourceRequest是iOS9的新特性，主要用于按需加载资源的下载控制。按需加载资源是由App Store托管的内容，它和下载的app bundle是分开的。app请求一系列按需加载资源，而下载和存储资源是由操作系统来管理。这些资源可以是除可执行代码外，bundle支持的任何类型。

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

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016031501.png)

可以用如下方式快速创建tag

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016031502.png)

在Resource Tags选项卡的Prefetched界面下，可以把tag分配给三个预获取优先级分类的其中一个。

- 初始安装tag（Initial install tags）。只有在初始安装tag下载到设备后，app才能启动。这些资源会在下载app时一起下载。这部分资源的大小会包括在App Store中app的安装包大小。如果这些资源从来没有被NSBundleResourceRequest对象获取过，就有可能被清理掉。
- 按顺序预获取tag（Prefetch tag order）。在app安装后会开始下载tag。tag会按照此处指定的顺序来下载。 
- 按需下载（Dowloaded only on demand）。当app请求一个tag，且tag没有缓存时，才会下载该tag。

# <a id="1">1 Initializing a Resource Request

```swift
/// 初始化NSBundleResourceRequest
///
/// - parameter tags : 相关tags
///
/// - returns: NSBundleResourceRequest
public convenience init(tags: Set<String>)
    
/// 初始化NSBundleResourceRequest
///
/// - parameter tags : 相关tags
/// - parameter bundle : 包
///
/// - returns: NSBundleResourceRequest
public init(tags: Set<String>, bundle: NSBundle)
```

# <a id="2">2 Accessing the Configuration

```swift
/// 加载的tags
public var tags: Set<String> { get }
/// 对应的NSBundle
public var bundle: NSBundle { get }
```

# <a id="3">3 Requesting Resources

```swift
// 开始加载资源
public func beginAccessingResourcesWithCompletionHandler(completionHandler: (NSError?) -> Void)
    
// 资源是否加载完成
public func conditionallyBeginAccessingResourcesWithCompletionHandler(completionHandler: (Bool) -> Void)
    
// 取消加载资源
public func endAccessingResources()
```

# <a id="4">4 Setting the Download Priority

```swift
/// 加载级别(0...1,默认0.5)
public var loadingPriority: Double
```

# <a id="5">5 Tracking Progress

```swift
// 资源加载管理器
public var progress: NSProgress { get }
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[NSBundleResourceRequest Class Reference](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSBundleResourceRequest_Class/index.html)

[On-Demand Resources Essentials](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html)

[NSBundle Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-15 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog