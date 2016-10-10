Certificate, Key, and Trust Services为管理证书、公钥私钥和信任策略提供了C语言接口。您可以在您的应用程序中使用这些服务：

1. 通过私钥确定身份；
2. 创建和请求证书；
3. 导入证书、密钥和身份；
4. 创建公钥-私钥对；
5. 代表信任策略。

**Concurrency Considerations**

在iOS上，该文章介绍的所有API都是线程安全的。


#1 Functions

##1.1 Getting Type Identifiers

```swift
// 获取SecCertificate的唯一ID
@available(iOS 2.0, *)
public func SecCertificateGetTypeID() -> CFTypeID
// SecIdentity的唯一ID
@available(iOS 2.0, *)
public func SecIdentityGetTypeID() -> CFTypeID
// SecKey的唯一ID
@available(iOS 2.0, *)
public func SecKeyGetTypeID() -> CFTypeID
// SecPolicy的唯一ID
@available(iOS 2.0, *)
public func SecPolicyGetTypeID() -> CFTypeID
// SecTrust的唯一ID
@available(iOS 2.0, *)
public func SecTrustGetTypeID() -> CFTypeID
```

##1.2 Managing Certificates

```swift
// 创建DER证书
@available(iOS 2.0, *)
public func SecCertificateCreateWithData(allocator: CFAllocator?, _ data: CFData) -> SecCertificate?
// 获取证书的CFData数据
@available(iOS 2.0, *)
public func SecCertificateCopyData(certificate: SecCertificate) -> CFData
// 获取证书的摘要
@available(iOS 2.0, *)
public func SecCertificateCopySubjectSummary(certificate: SecCertificate) -> CFString?
```

##1.3 Managing Identities

```swift
// 检索身份相关的证书
@available(iOS 2.0, *)
public func SecIdentityCopyCertificate(identityRef: SecIdentity, _ certificateRef: UnsafeMutablePointer<SecCertificate?>) -> OSStatus
// 检索身份相关的私钥
@available(iOS 2.0, *)
public func SecIdentityCopyPrivateKey(identityRef: SecIdentity, _ privateKeyRef: UnsafeMutablePointer<SecKey?>) -> OSStatus
// 通过PKCS数据获取身份和证书
@available(iOS 2.0, *)
public func SecPKCS12Import(pkcs12_data: CFData, _ options: CFDictionary, _ items: UnsafeMutablePointer<CFArray?>) -> OSStatus
```

##1.4 Cryptography and Digital Signatures

```swift
// 创建不对称密钥
@available(iOS 2.0, *)
public func SecKeyGeneratePair(parameters: CFDictionary, _ publicKey: UnsafeMutablePointer<SecKey?>, _ privateKey: UnsafeMutablePointer<SecKey?>) -> OSStatus
// 加密
@available(iOS 2.0, *)
public func SecKeyEncrypt(key: SecKey, _ padding: SecPadding, _ plainText: UnsafePointer<UInt8>, _ plainTextLen: Int, _ cipherText: UnsafeMutablePointer<UInt8>, _ cipherTextLen: UnsafeMutablePointer<Int>) -> OSStatus
// 解密
@available(iOS 2.0, *)
public func SecKeyDecrypt(key: SecKey, _ padding: SecPadding, _ cipherText: UnsafePointer<UInt8>, _ cipherTextLen: Int, _ plainText: UnsafeMutablePointer<UInt8>, _ plainTextLen: UnsafeMutablePointer<Int>) -> OSStatus
// 为一个data生成数字签名
@available(iOS 2.0, *)
public func SecKeyRawSign(key: SecKey, _ padding: SecPadding, _ dataToSign: UnsafePointer<UInt8>, _ dataToSignLen: Int, _ sig: UnsafeMutablePointer<UInt8>, _ sigLen: UnsafeMutablePointer<Int>) -> OSStatus
// 校验数字签名
@available(iOS 2.0, *)
public func SecKeyRawVerify(key: SecKey, _ padding: SecPadding, _ signedData: UnsafePointer<UInt8>, _ signedDataLen: Int, _ sig: UnsafePointer<UInt8>, _ sigLen: Int) -> OSStatus
// 获取密钥块的长度
@available(iOS 2.0, *)
public func SecKeyGetBlockSize(key: SecKey) -> Int
```

##1.5 Managing Policies

```swift
// 获取政策属性字典
@available(iOS 7.0, *)
public func SecPolicyCopyProperties(policyRef: SecPolicy) -> CFDictionary
// 获取默认的X.509政策
@available(iOS 2.0, *)
public func SecPolicyCreateBasicX509() -> SecPolicy
// 创建SSL证书
@available(iOS 2.0, *)
public func SecPolicyCreateSSL(server: Bool, _ hostname: CFString?) -> SecPolicy
```

##1.6 Managing Trust

```swift
// 基于给定的证书和政策创建一个信任对象
@available(iOS 2.0, *)
public func SecTrustCreateWithCertificates(certificates: AnyObject, _ policies: AnyObject?, _ trust: UnsafeMutablePointer<SecTrust?>) -> OSStatus
// 设置信任应该被验证的策略
@available(iOS 6.0, *)
public func SecTrustSetPolicies(trust: SecTrust, _ policies: AnyObject) -> OSStatus
// 返回用于此评估的策略数组
@available(iOS 7.0, *)
public func SecTrustCopyPolicies(trust: SecTrust, _ policies: UnsafeMutablePointer<CFArray?>) -> OSStatus
// 是否允许从网络中获取丢失的证书
@available(iOS 7.0, *)
public func SecTrustSetNetworkFetchAllowed(trust: SecTrust, _ allowFetch: Bool) -> OSStatus
// 是否允许信任从网络中获取的证书
@available(iOS 7.0, *)
public func SecTrustGetNetworkFetchAllowed(trust: SecTrust, _ allowFetch: UnsafeMutablePointer<DarwinBoolean>) -> OSStatus
// 为SecTrust设置证书
@available(iOS 2.0, *)
public func SecTrustSetAnchorCertificates(trust: SecTrust, _ anchorCertificates: CFArray) -> OSStatus
// 恢复除了SecTrustSetAnchorCertificates方法设置的证书
@available(iOS 2.0, *)
public func SecTrustSetAnchorCertificatesOnly(trust: SecTrust, _ anchorCertificatesOnly: Bool) -> OSStatus
// 获取SecTrustSetAnchorCertificates设置的证书
@available(iOS 7.0, *)
public func SecTrustCopyCustomAnchorCertificates(trust: SecTrust, _ anchors: UnsafeMutablePointer<CFArray?>) -> OSStatus
// 设置证书需要的校验时间
@available(iOS 2.0, *)
public func SecTrustSetVerifyDate(trust: SecTrust, _ verifyDate: CFDate) -> OSStatus
// 获取证书校验时间
@available(iOS 2.0, *)
public func SecTrustGetVerifyTime(trust: SecTrust) -> CFAbsoluteTime
// 同步同步证书
@available(iOS 2.0, *)
public func SecTrustEvaluate(trust: SecTrust, _ result: UnsafeMutablePointer<SecTrustResultType>) -> OSStatus
// 异步同步证书
@available(iOS 7.0, *)
public func SecTrustEvaluateAsync(trust: SecTrust, _ queue: dispatch_queue_t?, _ result: SecTrustCallback) -> OSStatus
// 获取证书信任评估
@available(iOS 7.0, *)
public func SecTrustGetTrustResult(trust: SecTrust, _ result: UnsafeMutablePointer<SecTrustResultType>) -> OSStatus
// 获取证书的公钥
@available(iOS 2.0, *)
public func SecTrustCopyPublicKey(trust: SecTrust) -> SecKey?
// 获取证书数量
@available(iOS 2.0, *)
public func SecTrustGetCertificateCount(trust: SecTrust) -> CFIndex
// 从信任链获取指定位置的证书
@available(iOS 2.0, *)
public func SecTrustGetCertificateAtIndex(trust: SecTrust, _ ix: CFIndex) -> SecCertificate?
// 获取证书cookie
@available(iOS 4.0, *)
public func SecTrustCopyExceptions(trust: SecTrust) -> CFData
// 设置证书cookie
@available(iOS 4.0, *)
public func SecTrustSetExceptions(trust: SecTrust, _ exceptions: CFData) -> Bool
// 获取证书属性数组
@available(iOS 2.0, *)
public func SecTrustCopyProperties(trust: SecTrust) -> CFArray?
// 返回包含证书链的字典
@available(iOS 7.0, *)
public func SecTrustCopyResult(trust: SecTrust) -> CFDictionary?
// 设置OCSPResponse到证书
@available(iOS 7.0, *)
public func SecTrustSetOCSPResponse(trust: SecTrust, _ responseData: AnyObject?) -> OSStatus
```

#2 Data Types

```swift
// X.509证书
public typealias SecCertificateRef = SecCertificate
public class SecCertificate {
}

// 身份对象
public typealias SecIdentityRef = SecIdentity
public class SecIdentity {
}

// 非对称秘钥
public typealias SecKeyRef = SecKey
public class SecKey {
}

// X.509有关策略信息
public typealias SecPolicyRef = SecPolicy
public class SecPolicy {
}

// 访问控制
public typealias SecAccessControlRef = SecAccessControl
public class SecAccessControl {
}

// X.509证书信任的评估
public typealias SecTrustRef = SecTrust
public class SecTrust {
}

// SecTrustEvaluateAsync调用结果
public typealias SecTrustCallback = (SecTrust, SecTrustResultType) -> Void
```

&#160;

----------

#Appendix

##Related Documentation

[Certificate, Key, and Trust Services Reference](https://developer.apple.com/library/ios/documentation/Security/Reference/certifkeytrustservices/index.html)

[Certificate, Key, and Trust Services Programming Guide](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CertKeyTrustProgGuide/iPhone_Tasks/iPhone_Tasks.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-07-31 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974