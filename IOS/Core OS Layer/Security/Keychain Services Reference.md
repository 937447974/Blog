Keychain Services的相关接口可以让你发现、增加、修改和删除钥匙串中的items。

使用OS X的钥匙链访问密码保护服务如下所示：

![](https://developer.apple.com/library/ios/documentation/Security/Conceptual/keychainServConcepts/art/unlocking_keychain.jpg)

使用iPhone访问网络服务器密钥链服务如下所示：

![](https://developer.apple.com/library/ios/documentation/Security/Conceptual/keychainServConcepts/art/iphone_keychain_flowchart.png)

#1 Functions
##1.1 Using Keychain Item Search Dictionaries

钥匙串由CFDictionary定义键值对。

```swift
// 搜索查询
@available(iOS 2.0, *)
public func SecItemCopyMatching(query: CFDictionary, _ result: UnsafeMutablePointer<AnyObject?>) -> OSStatus
// 增加
@available(iOS 2.0, *)
public func SecItemAdd(attributes: CFDictionary, _ result: UnsafeMutablePointer<AnyObject?>) -> OSStatus
// 修改
@available(iOS 2.0, *)
public func SecItemUpdate(query: CFDictionary, _ attributesToUpdate: CFDictionary) -> OSStatus
// 删除
@available(iOS 2.0, *)
public func SecItemDelete(query: CFDictionary) -> OSStatus
```

##1.2 Creating Access Control Objects

```swift
// 创建一个新的访问控制对象，该对象具有指定的保护类型和标志。
@available(iOS 8.0, *)
public func SecAccessControlCreateWithFlags(allocator: CFAllocator?, _ protection: AnyObject, _ flags: SecAccessControlCreateFlags, _ error: UnsafeMutablePointer<Unmanaged<CFError>?>) -> SecAccessControl?
```

#2 Constants
##2.1 OS X Keychain Services API Constants

```swift
// 预定义的关键常量时,基于字典的参数使用传递导入/导出功能
@available(iOS 2.0, *)
public let kSecImportExportPassphrase: CFString
```

##2.2 Keychain Item Class Keys and Values

```swift
// 搜索词典条目
@available(iOS 2.0, *)
public let kSecClass: CFString

// 一般密码
@available(iOS 2.0, *)
public let kSecClassGenericPassword: CFString
// 互联网密码
@available(iOS 2.0, *)
public let kSecClassInternetPassword: CFString
// 证书对象
@available(iOS 2.0, *)
public let kSecClassCertificate: CFString
// 专用秘钥
@available(iOS 2.0, *)
public let kSecClassKey: CFString
// 身份对象，包含kSecClassKey和kSecClassCertificate.
@available(iOS 2.0, *)
public let kSecClassIdentity: CFString
```

// http://blog.sina.com.cn/s/blog_7ea0400d0101fksj.html

##2.3 Attribute Item Keys and Values

```swift
```

##2.4 Search Keys

```swift
```

##2.5 Search Results Constants

```swift
```

##2.6 Access Control Create Flags

```swift
```

&#160;

----------

#Appendix

##Related Documentation

[Keychain Services Reference](https://developer.apple.com/library/ios/documentation/Security/Reference/keychainservices/index.html)

[Keychain Services Programming Guide](https://developer.apple.com/library/ios/documentation/Security/Reference/keychainservices/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-20 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974