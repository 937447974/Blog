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

###2.2.1 Item Class Key Constant

```swift
// 搜索词典条目
@available(iOS 2.0, *)
public let kSecClass: CFString
```

###2.2.2 Item Class Value Constants

```swift
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

##2.3 Attribute Item Keys and Values

###2.3.1 Attribute Item Keys

每种类型的钥匙串项可以有多个描述属性

| CFTypeRef | Declaration | value | readonly | kSecClassGenericPassword | kSecClassInternetPassword | kSecClassCertificate | kSecClassKey | kSecClassIdentity |
| --------  | ----------- | ----- | :------: | :----------------------: | :-----------------------: | :------------------: | :----------: | :---------------: |
| kSecAttrAccessible | 可访问性类型透明 | CFTypeRef | | √ | √ | √ | √ | √ |
| kSecAttrAccessControl(iOS 8.0) | 访问控制 | SecAccessControl | | √ | √ | √ | √ | √ |
| kSecAttrAccessGroup | 访问组 | CFStringRef | | √ | √ | √ | √ | √ |
| kSecAttrSynchronizable(iOS 7.0) | 数据同步或异步到其他设备 | CFBooleanRef | | √ | √ | √ | √ | √ |
| kSecAttrCreationDate | 创建日期 | CFDateRef | √ | √ | √ | | | |
| kSecAttrModificationDate | 最后一次修改日期 | CFDateRef | √ | √ | √ | | | |
| kSecAttrDescription | 描述 | CFStringRef | | √ | √ | | | |
| kSecAttrComment | 注释 | CFStringRef | | √ | √ | | | |
| kSecAttrCreator | 创造者 | CFNumberRef | | √ | √ | | | |
| kSecAttrType | 类型 | CFNumberRef | | √ | √ | | | |
| kSecAttrLabel | 标签 | CFStringRef | | √ | √ | √ | √ | √ |
| kSecAttrIsInvisible | 是否隐藏 | kCFBooleanTrue | | √ | √ | | | |
| kSecAttrIsNegative | 是否具有密码 | CFBooleanRef | | √ | √ | | | |
| kSecAttrAccount | 账户 | CFStringRef | | √ | √ | | | |
| kSecAttrService | 所具有服务 | CFStringRef | | √ | | | | |
| kSecAttrGeneric | 用户自定义内容 | CFDataRef | | √ | | | | |
| kSecAttrSecurityDomain | 网络安全域 | CFStringRef | | | √ | | | |
| kSecAttrServer | 服务器域名或IP地址 | CFStringRef | | | √ | | | |
| kSecAttrProtocol | 协议 | CFNumberRef | | | √ | | | |
| kSecAttrAuthenticationType | 认证类型 | CFNumberRef | | | √ | | | |
| kSecAttrPort | 网络端口 | CFNumberRef | | | √ | | | |
| kSecAttrPath | 访问路径 | CFStringRef | | | √ | | | |
| kSecAttrSubject | X.500证书主题名称 | CFDataRef | √ | | | √ | | √ |
| kSecAttrIssuer | X.500证书颁发者名称 | CFDataRef | √ | | | √ | | √ |
| kSecAttrSerialNumber | 序列号 | CFDataRef | √ | | | √ | | √ |
| kSecAttrSubjectKeyID | 主题ID | CFDataRef | √ | | | √ | | √ |
| kSecAttrPublicKeyHash | 公钥Hash值 | CFDataRef | √ | | | √ | | √ |
| kSecAttrCertificateType | 证书类型 | CFNumberRef | √ | | | √ | | √ |
| kSecAttrCertificateEncoding | 证书编码类型 | CFNumberRef | √ | | | √ | | √ |
| kSecAttrKeyClass | 加密密钥类 | CFTypeRef | √ | | | | √ | √ |
| kSecAttrApplicationLabel | 标签(给程序使用) | CFStringRef | | | | | √ | √ |
| kSecAttrIsPermanent | 是否永久保存加密密钥  | CFBooleanRef | | | | | √ | √ |
| kSecAttrApplicationTag | 标签(私有标签数据) | CFDataRef | | | | | √ | √ |
| kSecAttrKeyType | 加密密钥类型(算法) | CFNumberRef | | | | | √ | √ |
| kSecAttrKeySizeInBits | 密钥总位数 | CFNumberRef | | | | | √ | √ |
| kSecAttrEffectiveKeySize | 密钥有效位数 | CFNumberRef | | | | | √ | √ |
| kSecAttrCanEncrypt | 密钥是否可用于加密 | CFBooleanRef | | | | | √ | √ |
| kSecAttrCanDecrypt | 密钥是否可用于解密 | CFBooleanRef | | | | | √ | √ |
| kSecAttrCanDerive | 密钥是否可用于导出其他密钥 | CFBooleanRef | | | | | √ | √ |
| kSecAttrCanSign | 密钥是否可用于数字签名 | CFBooleanRef | | | | | √ | √ |
| kSecAttrCanVerify | 密钥是否可用于验证数字签名 | CFBooleanRef | | | | | √ | √ |
| kSecAttrCanWrap | 密钥是否可用于打包其他密钥 | CFBooleanRef | | | | | √ | √ |
| kSecAttrCanUnwrap | 密钥是否可用于解包其他密钥 | CFBooleanRef | | | | | √ | √ |
| kSecAttrSyncViewHint(iOS 9.0) | 同步视图中的定义查询 | CFStringRef | | | | | | |
| kSecAttrTokenID(iOS 9.0) | 令牌 | CFStringRef | | | | | | |

>1. kSecAttrAccessGroup：如果希望这个keychain的item可以被多个应用share，可以给这个item设置这个属性，类型是CFStringRef。应用程序在被编译时，可以在entitlement中指定自己的accessgroup，如果应用的accessgroup名字和keychain item的accessgroup名字一致，那这个应用就可以访问这个item，不过这个设计并不是很好，因为应用的accessgroup是由应用开发者指定的，它可以故意跟其他应用的accessgroup一样，从而访问其他应用的item，更可怕的是还支持wildcard，比如keychain-dumper将自己的accessgroup指定为*，从而可以把keychain中的所有item都dump出来。
>2. kSecAttrTokenID: 当前对应的值只有kSecAttrTokenIDSecureEnclave

###2.3.2 Protocol Values

kSecAttrProtocol对应的values

```swift
let kSecAttrProtocolFTP: CFString // FTP protocol.
let kSecAttrProtocolFTPAccount: CFString // A client side FTP account.
let kSecAttrProtocolHTTP: CFString // HTTP protocol.
let kSecAttrProtocolIRC: CFString // IRC protocol.
let kSecAttrProtocolNNTP: CFString // NNTP protocol.
let kSecAttrProtocolPOP3: CFString // POP3 protocol.
let kSecAttrProtocolSMTP: CFString // SMTP protocol.
let kSecAttrProtocolSOCKS: CFString // SOCKS protocol.
let kSecAttrProtocolIMAP: CFString // IMAP protocol.
let kSecAttrProtocolLDAP: CFString // LDAP protocol.
let kSecAttrProtocolAppleTalk: CFString // AFP over AppleTalk.
let kSecAttrProtocolAFP: CFString // AFP over TCP.
let kSecAttrProtocolTelnet: CFString // Telnet protocol.
let kSecAttrProtocolSSH: CFString // SSH protocol.
let kSecAttrProtocolFTPS: CFString // FTP over TLS/SSL.
let kSecAttrProtocolHTTPS: CFString // HTTP over TLS/SSL.
let kSecAttrProtocolHTTPProxy: CFString // HTTP proxy.
let kSecAttrProtocolHTTPSProxy: CFString // HTTPS proxy.
let kSecAttrProtocolFTPProxy: CFString // FTP proxy.
let kSecAttrProtocolSMB: CFString // SMB protocol.
let kSecAttrProtocolRTSP: CFString // RTSP protocol.
let kSecAttrProtocolRTSPProxy: CFString // RTSP proxy.
let kSecAttrProtocolDAAP: CFString // DAAP protocol.
let kSecAttrProtocolEPPC: CFString // Remote Apple Events.
let kSecAttrProtocolIPP: CFString // IPP protocol.
let kSecAttrProtocolNNTPS: CFString // NNTP over TLS/SSL.
let kSecAttrProtocolLDAPS: CFString // LDAP over TLS/SSL.
let kSecAttrProtocolTelnetS: CFString // Telnet over TLS/SSL.
let kSecAttrProtocolIMAPS: CFString // IMAP over TLS/SSL.
let kSecAttrProtocolIRCS: CFString // IRC over TLS/SSL.
let kSecAttrProtocolPOP3S: CFString // POP3 over TLS/SSL.
```

###2.3.3 Authentication Type Values

kSecAttrAuthenticationType对应的values

```swift
let kSecAttrAuthenticationTypeNTLM: CFString // Windows NT LAN Manager authentication.
let kSecAttrAuthenticationTypeMSN: CFString // Microsoft Network default authentication.
let kSecAttrAuthenticationTypeDPA: CFString // Distributed Password authentication.
let kSecAttrAuthenticationTypeRPA: CFString // Remote Password authentication.
let kSecAttrAuthenticationTypeHTTPBasic: CFString // HTTP Basic authentication.
let kSecAttrAuthenticationTypeHTTPDigest: CFString // HTTP Digest Access authentication.
let kSecAttrAuthenticationTypeHTMLForm: CFString // HTML form based authentication.
let kSecAttrAuthenticationTypeDefault: CFString // The default authentication type.
```

###2.3.4 Key Class Values

kSecAttrKeyClass对应的values

```swift
let kSecAttrKeyClassPublic: CFString // 公钥 
let kSecAttrKeyClassPrivate: CFString // 私钥
let kSecAttrKeyClassSymmetric: CFString // 对称密钥
```

###2.3.5 Key Type Values

kSecAttrKeyType对应的values

```swift
let kSecAttrKeyTypeRSA: CFString // RSA公钥加密算法
let kSecAttrKeyTypeEC: CFString // 非对称加密
```

###2.3.6 Keychain Item Accessibility Constants

kSecAttrAccessible对应的常量，默认kSecAttrAccessibleWhenUnlocked

```swift
let kSecAttrAccessibleWhenUnlocked: CFString // 解锁可访问，加密备份
let kSecAttrAccessibleAfterFirstUnlock: CFString // 设备重启、第一次解锁后可访问，加密备份
let kSecAttrAccessibleAlways: CFString // 一直可访问，加密备份
@available(iOS 8.0, *)
let kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly: CFString // 设备解锁时才被访问，不备份，禁用设备密码会导致这类项目被删除。
let kSecAttrAccessibleWhenUnlockedThisDeviceOnly: CFString // 解锁可访问，不备份
let kSecAttrAccessibleAfterFirstUnlockThisDeviceOnly: CFString // 设备重启、第一次解锁后可访问，不备份
let kSecAttrAccessibleAlwaysThisDeviceOnly: CFString // 一直可访问，不备份
```

###2.3.7 kSecAttrSynchronizable Value Constants

使用于SecItemCopyMatching, SecItemUpdate, or SecItemDelete.

```swift
@available(iOS 7.0, *)
public let kSecAttrSynchronizableAny: CFString // 同步和非同步返回查询结果
```

###2.3.8 kSecAttrTokenID Value Constants

使用kSecAttrKeyTypeEC 256-bits加密，对应使用的kSecAttrTokenID和kSecAttrTokenIDSecureEnclave

```swift
@available(iOS 9.0, *)
public let kSecAttrTokenIDSecureEnclave: CFString // 秘钥
```

##2.4 Search Keys

###2.4.1 Search Attribute Keys

查询时使用的属性key

```swift
let kSecMatchPolicy: CFString // 指定策略
let kSecMatchItemList: CFString // 指定搜索范围 CFArrayRef(SecKeychainItemRef, SecKeyRef, SecCertificateRef, SecIdentityRef,CFDataRef)数组内的类型必须唯一。仍然会搜索钥匙串，但是搜索结果需要与该数组取交集作为最终结果。
let kSecMatchSearchList: CFString // 搜索列表  CFArray
let kSecMatchIssuers: CFString // 指定发行人数组 CFArrayRef(kSecAttrIssuer对应的value)
let kSecMatchEmailAddressIfPresent: CFString // 指定邮件地址 CFStringRef
let kSecMatchSubjectContains: CFString // 指定主题 CFStringRef
let kSecMatchCaseInsensitive: CFString // 指定是否不区分大小写 CFBooleanRef(kCFBooleanFalse或不提供此参数,区分大小写;kCFBooleanTrue,不区分大小写)
let kSecMatchTrustedOnly: CFString // 指定只搜索可信证书 CFBooleanRef(kCFBooleanFalse或不提供此参数,全部证书;kCFBooleanTrue,只搜索可信证书)
let kSecMatchValidOnDate: CFString // 指定有效日期 CFDateRef(kCFNull表示今天)
let kSecMatchLimit: CFString // 指定结果数量 CFNumberRef(kSecMatchLimitOne or kSecMatchLimitAll)
let kSecMatchLimitOne: CFString // 首条结果
let kSecMatchLimitAll: CFString // 全部结果
```

###2.4.2 Item List Key

用于指定要搜索或添加的项目列表的键。用户提供用于查询的列表。当这个列表被提供的时候，不会再搜索钥匙串。

```swift
let kSecUseItemList: CFString // CFArrayRef(SecKeychainItemRef, SecKeyRef, SecCertificateRef, SecIdentityRef, or (for persistent item references) CFDataRef items. )
```

##2.5 Search Results Constants

###2.5.1 Return Type Keys

搜索的返回值

```swift
let kSecReturnData: CFString // 返回数据(CFDataRef) CFBooleanRef
let kSecReturnAttributes: CFString // 返回属性字典(CFDictionaryRef) CFBooleanRef
let kSecReturnRef: CFString // 返回实例(SecKeychainItemRef, SecKeyRef, SecCertificateRef, SecIdentityRef, or CFDataRef) CFBooleanRef
let kSecReturnPersistentRef: CFString // 返回持久型实例(CFDataRef) CFBooleanRef
```

###2.5.2 Value Type Keys

```swift
let kSecValueData: CFString // data数据(CFDataRef)
let kSecValueRef: CFString // 引用数据（SecKeychainItemRef, SecKeyRef, SecCertificateRef, or SecIdentityRef.）
let kSecValuePersistentRef: CFString // 强引用数据（CFDataRef）
```

##2.6 Access Control Create Flags

SecAccessControlCreateFlags方法使用的常数

```swift
@available(iOS 8.0, *)
public struct SecAccessControlCreateFlags : OptionSetType {
    public init(rawValue: CFIndex)
    
    public static var UserPresence: SecAccessControlCreateFlags { get } // User presence policy using Touch ID or Passcode. Touch ID does not have to be available or enrolled. Item is still accessible by Touch ID even if fingers are added or removed.
    @available(iOS 9.0, *)
    public static var TouchIDAny: SecAccessControlCreateFlags { get } // Constraint: Touch ID (any finger). Touch ID must be available and at least one finger must be enrolled. Item is still accessible by Touch ID even if fingers are added or removed.
    @available(iOS 9.0, *)
    public static var TouchIDCurrentSet: SecAccessControlCreateFlags { get } // Constraint: Touch ID from the set of currently enrolled fingers. Touch ID must be available and at least one finger must be enrolled. When fingers are added or removed, the item is invalidated.
    @available(iOS 9.0, *)
    public static var DevicePasscode: SecAccessControlCreateFlags { get } // Constraint: Device passcode
    @available(iOS 9.0, *)
    public static var Or: SecAccessControlCreateFlags { get } // Constraint logic operation: when using more than one constraint, at least one of them must be satisfied.
    @available(iOS 9.0, *)
    public static var And: SecAccessControlCreateFlags { get } // Constraint logic operation: when using more than one constraint, all must be satisfied.
    @available(iOS 9.0, *)
    public static var PrivateKeyUsage: SecAccessControlCreateFlags { get } // Create access control for private key operations (i.e. sign operation)
    @available(iOS 9.0, *)
    public static var ApplicationPassword: SecAccessControlCreateFlags { get } // Security: Application provided password for data encryption key generation. This is not a constraint but additional item encryption mechanism.
}
```

##2.7 Other Constants

##2.7.1 predefined constants

```swift
@available(iOS 8.0, *)
public let kSecUseOperationPrompt: CFString // UI校验通过
@available(iOS 9.0, *)
public let kSecUseAuthenticationUI: CFString // 验证UI（CFBooleanRef)
@available(iOS 9.0, *)
public let kSecUseAuthenticationContext: CFString // 秘钥item验证(LAContext)
```

###2.7.2 kSecUseAuthenticationUI Value Constants

```swift
@available(iOS 9.0, *)
public let kSecUseAuthenticationUIAllow: CFString // UI校验通过
@available(iOS 9.0, *)
public let kSecUseAuthenticationUIFail: CFString // UI校验出错
@available(iOS 9.0, *)
public let kSecUseAuthenticationUISkip: CFString // UI校验跳过
```

&#160;

----------

#Appendix

##Related Documentation

[Keychain Services Reference](https://developer.apple.com/library/ios/documentation/Security/Reference/keychainservices/index.html)

[Keychain Services Programming Guide](https://developer.apple.com/library/ios/documentation/Security/Reference/keychainservices/index.html)

[iOS钥匙串KeyChain相关参数的说明](http://blog.sina.com.cn/s/blog_7ea0400d0101fksj.html)

[iOS 再谈Keychain钥匙串，应用间数据共享打造iOS上的全家桶](http://www.mamicode.com/info-detail-1159388.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-07-28 | 博文开始 |
| 2016-07-29 | 博文完成 |


##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974