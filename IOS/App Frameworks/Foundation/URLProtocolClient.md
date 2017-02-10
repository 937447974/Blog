URLProtocolClient主要用于对自定义的NSURLProtocol子类提供URL数据交互接口。

#Symbols

##1 Protocol Methods

```swift
// 重定向请求和返回数据
public func urlProtocol(_ protocol: URLProtocol, wasRedirectedTo request: URLRequest, redirectResponse: URLResponse)
    
// 校验缓存是有效的
public func urlProtocol(_ protocol: URLProtocol, cachedResponseIsValid cachedResponse: CachedURLResponse)

// 设置请求回调数据的缓存策略
public func urlProtocol(_ protocol: URLProtocol, didReceive response: URLResponse, cacheStoragePolicy policy: URLCache.StoragePolicy)
// 回调请求对应的数据
public func urlProtocol(_ protocol: URLProtocol, didLoad data: Data)

// 加载完毕
public func urlProtocolDidFinishLoading(_ protocol: URLProtocol)
// 加载出错
public func urlProtocol(_ protocol: URLProtocol, didFailWithError error: Error)
    
// 启动身份验证
public func urlProtocol(_ protocol: URLProtocol, didReceive challenge: URLAuthenticationChallenge)
// 取消证书验证
public func urlProtocol(_ protocol: URLProtocol, didCancel challenge: URLAuthenticationChallenge)
```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[URLProtocolClient](https://developer.apple.com/reference/foundation/urlprotocolclient)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-02-10 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974