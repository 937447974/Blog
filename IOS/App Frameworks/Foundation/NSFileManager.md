1. [Creating a File Manager](#1)
2. [Locating System Directories](#2)
3. [Locating Application Group Container Directories](#3)
4. [Discovering Directory Contents](#4)
5. [Creating and Deleting Items](#5)
6. [Moving and Copying Items](#6)
7. [Managing iCloud-Based Items](#7)
8. [Creating Symbolic and Hard Links](#8)
9. [Determining Access to Files](#9)
10. [Getting and Setting Attributes](#10)
11. [Getting and Comparing File Contents](#11)
12. [Getting the Relationship Between Items](#12)
13. [Converting File Paths to Strings](#13)
14. [Managing the Delegate](#14)
15. [Managing the Current Directory](#15)
16. [Deprecated Methods](#16)

----

NSFileManager(Swift中是FileManager)帮助我们快速访问管理文件系统。通过它我们可以对文件进行定位、复制、移动和删除等操作。

# <a id="1">1 Creating a File Manager

```swift
/// 获取共享的NSFileManager
public class func defaultManager() -> NSFileManager
```

# <a id="2">2 Locating System Directories

```swift
// 快速获取目录
@available(iOS 4.0, *)
public func URLsForDirectory(directory: NSSearchPathDirectory, inDomains domainMask: NSSearchPathDomainMask) -> [NSURL]
    
/// 文件夹内快速创建文件夹
///
/// - parameter directory : .ItemReplacementDirectory
/// - parameter inDomain : .UserDomainMask
/// - parameter appropriateForURL : 目标目录
/// - parameter create : 是否创建
///
/// - returns: void
@available(iOS 4.0, *)
public func URLForDirectory(directory: NSSearchPathDirectory, inDomain domain: NSSearchPathDomainMask, appropriateForURL url: NSURL?, create shouldCreate: Bool) throws -> NSURL
```

# <a id="3">3 Locating Application Group Container Directories

```swift
/// 获取应用容器目录
/// 定位应用组目录，需在https://idmsa.apple.com/IDMSWebAuth/authenticate配置
///
/// - parameter groupIdentifier : 容器ID
///
/// - returns: NSURL?
@available(iOS 7.0, *)
public func containerURLForSecurityApplicationGroupIdentifier(groupIdentifier: String) -> NSURL?
```

# <a id="4">4 Discovering Directory Contents

```swift
/// 获取一级目录内的内容
///
/// - parameter url : 目标路径
/// - parameter includingPropertiesForKeys : 关键字
/// - parameter options : 枚举选项，NSDirectoryEnumerationOptions.SkipsHiddenFiles
///
/// - returns: [NSURL]
@available(iOS 4.0, *)
public func contentsOfDirectoryAtURL(url: NSURL, includingPropertiesForKeys keys: [String]?, options mask: NSDirectoryEnumerationOptions) throws -> [NSURL] 
@available(iOS 2.0, *)
public func contentsOfDirectoryAtPath(path: String) throws -> [String]

/// 枚举获取目录下内容
@available(iOS 4.0, *)
public func enumeratorAtURL(url: NSURL, includingPropertiesForKeys keys: [String]?, options mask: NSDirectoryEnumerationOptions, errorHandler handler: ((NSURL, NSError) -> Bool)?) -> NSDirectoryEnumerator?
public func enumeratorAtPath(path: String) -> NSDirectoryEnumerator?

/// 返回所有路径
public func subpathsAtPath(path: String) -> [String]?
@available(iOS 2.0, *)
public func subpathsOfDirectoryAtPath(path: String) throws -> [String]
```

# <a id="5">5 Creating and Deleting Items

```swift
/// 创建文件夹
@available(iOS 5.0, *)
public func createDirectoryAtURL(url: NSURL, withIntermediateDirectories createIntermediates: Bool, attributes: [String : AnyObject]?) throws
@available(iOS 2.0, *)
public func createDirectoryAtPath(path: String, withIntermediateDirectories createIntermediates: Bool, attributes: [String : AnyObject]?) throws
    
/// 创建文件
public func createFileAtPath(path: String, contents data: NSData?, attributes attr: [String : AnyObject]?) -> Bool
    
/// 删除文件
@available(iOS 4.0, *)
public func removeItemAtURL(URL: NSURL) throws
@available(iOS 2.0, *)
public func removeItemAtPath(path: String) throws
    
/// 替换数据内容
@available(iOS 4.0, *)
public func replaceItemAtURL(originalItemURL: NSURL, withItemAtURL newItemURL: NSURL, backupItemName: String?, options: NSFileManagerItemReplacementOptions, resultingItemURL resultingURL: AutoreleasingUnsafeMutablePointer<NSURL?>) throws
```

# <a id="6">6 Moving and Copying Items

```swift
/// 复制文件
@available(iOS 2.0, *)
public func copyItemAtPath(srcPath: String, toPath dstPath: String) throws
@available(iOS 4.0, *)
public func copyItemAtURL(srcURL: NSURL, toURL dstURL: NSURL) throws
    
// 移动文件
@available(iOS 2.0, *)
public func moveItemAtPath(srcPath: String, toPath dstPath: String) throws
@available(iOS 4.0, *)
public func moveItemAtURL(srcURL: NSURL, toURL dstURL: NSURL) throws
```

# <a id="7">7 Managing iCloud-Based Items

```swift
/// 获取iCloud身份标识符
@available(iOS 6.0, *)
@NSCopying public var ubiquityIdentityToken: protocol<NSCoding, NSCopying, NSObjectProtocol>? { get }

// 根据iCloud容器标识符，创建容器路径
@available(iOS 5.0, *)
public func URLForUbiquityContainerIdentifier(containerIdentifier: String?) -> NSURL?

// 目标路径是否存储在iCloud
@available(iOS 5.0, *)
public func isUbiquitousItemAtURL(url: NSURL) -> Bool

/// 移动文件到云中
///
/// - parameter flag : true移动到云中,false从云中删除
/// - parameter itemAtURL : 本地路径
/// - parameter destinationURL : 云路径
///
/// - returns: void
@available(iOS 5.0, *)
public func setUbiquitous(flag: Bool, itemAtURL url: NSURL, destinationURL: NSURL) throws

/// 从云中下载文件
@available(iOS 5.0, *)
public func startDownloadingUbiquitousItemAtURL(url: NSURL) throws

// 删除云对应的本地副本
@available(iOS 5.0, *)
public func evictUbiquitousItemAtURL(url: NSURL) throws

/// 生成云的url，可在浏览器中下载
@available(iOS 5.0, *)
public func URLForPublishingUbiquitousItemAtURL(url: NSURL, expirationDate outDate: AutoreleasingUnsafeMutablePointer<NSDate?>) throws -> NSURL
```

# <a id="8">8 Creating Symbolic and Hard Links

```swift
// 创建软连接
@available(iOS 2.0, *)
open func createSymbolicLink(atPath path: String, withDestinationPath destPath: String) throws
@available(iOS 5.0, *)
open func createSymbolicLink(at url: URL, withDestinationURL destURL: URL) throws

// 创建硬连接
@available(iOS 2.0, *)
open func linkItem(atPath srcPath: String, toPath dstPath: String) throws
@available(iOS 4.0, *)
open func linkItem(at srcURL: URL, to dstURL: URL) throws

// 获取软连接对应的实际地址
@available(iOS 2.0, *)
open func destinationOfSymbolicLink(atPath path: String) throws -> String
```

# <a id="9">9 Determining Access to Files

```swift
// 路径对应的文件或文件夹是否存在
open func fileExists(atPath path: String) -> Bool  
// 路径对应的文件或文件夹是否存在，isDirectory对应是否为目录
open func fileExists(atPath path: String, isDirectory: UnsafeMutablePointer<ObjCBool>?) -> Bool   
// 能否读取文件
open func isReadableFile(atPath path: String) -> Bool 
// 能否写入文件
open func isWritableFile(atPath path: String) -> Bool 
// 是否为可执行文件
open func isExecutableFile(atPath path: String) -> Bool
// 能否删除文件
open func isDeletableFile(atPath path: String) -> Bool
```

# <a id="10">10 Getting and Setting Attributes

```swift
// 文件名
open func displayName(atPath path: String) -> String
// 路径上的文件夹名
open func componentsToDisplay(forPath path: String) -> [String]?
// 获取文件属性
@available(iOS 2.0, *)
open func attributesOfItem(atPath path: String) throws -> [FileAttributeKey : Any]
// 获取文件所处系统的相关文件属性（如剩余存储空间）
@available(iOS 2.0, *)
open func attributesOfFileSystem(forPath path: String) throws -> [FileAttributeKey : Any]
// 设置文件属性
@available(iOS 2.0, *)
open func setAttributes(_ attributes: [FileAttributeKey : Any], ofItemAtPath path: String) throws
```

# <a id="11">11 Getting and Comparing File Contents

```swift
// 获取文件内容
open func contents(atPath path: String) -> Data?
// 判断文件内容是否相同
open func contentsEqual(atPath path1: String, andPath path2: String) -> Bool
```

# <a id="12">12 Getting the Relationship Between Items

```swift
// 获取路径之间的关系
@available(iOS 8.0, *)
open func getRelationship(_ outRelationship: UnsafeMutablePointer<FileManager.URLRelationship>, ofDirectoryAt directoryURL: URL, toItemAt otherURL: URL) throws
// 获取路径是否在某容器中
@available(iOS 8.0, *)
open func getRelationship(_ outRelationship: UnsafeMutablePointer<FileManager.URLRelationship>, of directory: FileManager.SearchPathDirectory, in domainMask: FileManager.SearchPathDomainMask, toItemAt url: URL) throws
```

# <a id="13">13 Converting File Paths to Strings

```swift
// 路径转C字符串
open func fileSystemRepresentation(withPath path: String) -> UnsafePointer<Int8>
// C字符串转路径
open func string(withFileSystemRepresentation str: UnsafePointer<Int8>, length len: Int) -> String    
```

# <a id="14">14 Managing the Delegate

```swift
// 代理监听
@available(iOS 2.0, *)
unowned(unsafe) open var delegate: FileManagerDelegate?    
```

# <a id="15">15 Managing the Current Directory

```swift
// 修改当前工作目录为指定目录
open func changeCurrentDirectoryPath(_ path: String) -> Bool
// 获取当前工作目录
open var currentDirectoryPath: String { get }
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[File System Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSFileManager_Class/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-14 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog