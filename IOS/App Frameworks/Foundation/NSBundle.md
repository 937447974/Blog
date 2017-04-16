1. [Initializing an NSBundle](#1)
2. [Getting an NSBundle](#2)
3. [Getting a Bundled Class](#3)
4. [Finding Resources](#4)
5. [Getting the Bundle Directory](#5)
6. [Getting Bundle Information](#6)
7. [Managing Localized Resources](#7)
8. [Loading a Bundle’s Code](#8)
9. [Managing Localizations](#9)
10. [Managing Preservation Priority for On Demand Resources](#10)

-----

NSBundle主要用于快速访问APP中的资源文件，如xib、plist、image等。

# <a id="1">1 Initializing an NSBundle

```swift
/// 根据路径生成NSBundle
public init?(path: String)
    
/// 根据路径生成NSBundle
@available(iOS 4.0, *)
public convenience init?(URL url: NSURL)
```

# <a id="2">2 Getting an NSBundle

```swift
/// 获取主bundle，Info.plist
public class func mainBundle() -> NSBundle

/// 根据类AnyClass关联获取bundle
public /*not inherited*/ init(forClass aClass: AnyClass)

/// 根据标识符获取bundle
public /*not inherited*/ init?(identifier: String)

/// 获取所有NSBundle
public class func allBundles() -> [NSBundle]

/// 获取framework
public class func allFrameworks() -> [NSBundle]
```

# <a id="3">3 Getting a Bundled Class

```swift
/// 根据class名生成class
public func classNamed(className: String) -> AnyClass?

/// 捆绑的主要类
public var principalClass: AnyClass? { get }
```

# <a id="4">4 Finding Resources

```swift
/// 源path
@available(iOS 4.0, *)
@NSCopying public var resourceURL: NSURL? { get }
public var resourcePath: String? { get }
/// 关联的appStore路径
@available(iOS 7.0, *)
@NSCopying public var appStoreReceiptURL: NSURL? { get }

/// 获取某个包下面的文件路径
///
/// - parameter name : 文件名
/// - parameter ext : 扩展，如plist
/// - parameter subpath : 搜索的目录
///
/// - returns: NSURL/String
@available(iOS 4.0, *)
public func URLForResource(name: String?, withExtension ext: String?, subdirectory subpath: String?) -> NSURL?
@available(iOS 4.0, *)
public func URLForResource(name: String?, withExtension ext: String?) -> NSURL?
public func pathForResource(name: String?, ofType ext: String?) -> String?
    
/// 获取包路径下指定文件名、文件类型的文件
/// 获取文件路径
///
/// - parameter name : 文件名
/// - parameter ext : 文件后缀名
/// - parameter subpath : 搜索的path目录
/// - parameter bundleURL : 文件包路径
///
/// - returns: NSURL/String
@available(iOS 4.0, *)
public class func URLForResource(name: String?, withExtension ext: String?, subdirectory subpath: String?, inBundleWithURL bundleURL: NSURL) -> NSURL?
@available(iOS 4.0, *)
public class func URLsForResourcesWithExtension(ext: String?, subdirectory subpath: String?, inBundleWithURL bundleURL: NSURL) -> [NSURL]?
public class func pathForResource(name: String?, ofType ext: String?, inDirectory bundlePath: String) -> String?
public func pathForResource(name: String?, ofType ext: String?, inDirectory subpath: String?) -> String?
public class func pathsForResourcesOfType(ext: String?, inDirectory bundlePath: String) -> [String]
    
/// 获取文件路径
///
/// - parameter name : 文件名
/// - parameter ext : 文件后缀名
/// - parameter subpath : 搜索的path目录
/// - parameter localizationName : 本地化语言ID
///
/// - returns: NSURL/String
@available(iOS 4.0, *)
public func URLForResource(name: String?, withExtension ext: String?, subdirectory subpath: String?, localization localizationName: String?) -> NSURL?
public func pathForResource(name: String?, ofType ext: String?, inDirectory subpath: String?, forLocalization localizationName: String?) -> String?
@available(iOS 4.0, *)
public func URLsForResourcesWithExtension(ext: String?, subdirectory subpath: String?, localization localizationName: String?) -> [NSURL]?
public func pathsForResourcesOfType(ext: String?, inDirectory subpath: String?, forLocalization localizationName: String?) -> [String]
@available(iOS 4.0, *)
public func URLsForResourcesWithExtension(ext: String?, subdirectory subpath: String?) -> [NSURL]?
public func pathsForResourcesOfType(ext: String?, inDirectory subpath: String?) -> [String]
```

# <a id="5">5 Getting the Bundle Directory

```swift
/// 包路径
public var bundlePath: String { get }
@available(iOS 4.0, *)
@NSCopying public var bundleURL: NSURL { get }
```

# <a id="6">6 Getting Bundle Information

```swift
// 包标示符Bundle ID
public var bundleIdentifier: String? { get }
// 对应的字典
public var infoDictionary: [String : AnyObject]? { get }
// 从字典中取数据
public func objectForInfoDictionaryKey(key: String) -> AnyObject?
    
/// 插件文件的路径
@available(iOS 4.0, *)
@NSCopying public var builtInPlugInsURL: NSURL? { get }
public var builtInPlugInsPath: String? { get }
    
/// 可执行的文件路径
@available(iOS 4.0, *)
@NSCopying public var executableURL: NSURL? { get }
public var executablePath: String? { get }
    
/// 规定文件名的可执行文件路径
@available(iOS 4.0, *)
public func URLForAuxiliaryExecutable(executableName: String) -> NSURL?
public func pathForAuxiliaryExecutable(executableName: String) -> String?
    
/// 私有库地址
@available(iOS 4.0, *)
@NSCopying public var privateFrameworksURL: NSURL? { get }
public var privateFrameworksPath: String? { get }
    
/// 共享库地址
@available(iOS 4.0, *)
@NSCopying public var sharedFrameworksURL: NSURL? { get }
 public var sharedFrameworksPath: String? { get }
    
/// 可共享的文件地址
@available(iOS 4.0, *)
@NSCopying public var sharedSupportURL: NSURL? { get }
public var sharedSupportPath: String? { get }
```

# <a id="7">7 Managing Localized Resources

```swift
/// 返回本地化字符串对应的值
public func localizedStringForKey(key: String, value: String?, table tableName: String?) -> String
```

# <a id="8">8 Loading a Bundle’s Code

```swift
/// 可执行的cpu架构
@available(iOS 2.0, *)
public var executableArchitectures: [NSNumber]? { get }
// 可执行代码能否加载成功
@available(iOS 2.0, *)
public func preflight() throws
// 动态加载可执行代码
public func load() -> Bool
// 加载过程中并返回错误
@available(iOS 2.0, *)
public func loadAndReturnError() throws
// 加载状态
public var loaded: Bool { get }
// 卸载可执行代码
public func unload() -> Bool
```

# <a id="9">9 Managing Localizations

```swift
// 从一个或多个本地化id中获取当前支持的id
public class func preferredLocalizationsFromArray(localizationsArray: [String]) -> [String]
public class func preferredLocalizationsFromArray(localizationsArray: [String], forPreferences preferencesArray: [String]?) -> [String]
// 优选本地化
public var preferredLocalizations: [String] { get }
// 开发语言
public var developmentLocalization: String? { get }
// 本地化字典
public var localizedInfoDictionary: [String : AnyObject]? { get }
// 本地化支持的ID
public var localizations: [String] { get }
```

# <a id="10">10 Managing Preservation Priority for On Demand Resources

```swift
/// 设置优先级
@available(iOS 9.0, *)
public func setPreservationPriority(priority: Double, forTags tags: Set<String>)
/// 获取优先级
@available(iOS 9.0, *)
public func preservationPriorityForTag(tag: String) -> Double
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[NSBundle Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html)

[Bundle Programming Guide](https://developer.apple.com/library/ios/documentation/CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html)

[Resource Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-14 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog