NSURLProtocol主要用于处理特定协议的数据加载。它本身是一个抽象类，通过继承它我们可以自定义任何网络协议来返回给app数据，还可以拦截非法请求。

# Symbols

## 1 Creating Protocol Objects

```swift
// 初始化
public init(request: URLRequest, cachedResponse: CachedURLResponse?, client: URLProtocolClient?)
```

## 2 Registering and Unregistering Protocol Classes

```swift
// 注册实现的子类
open class func registerClass(_ protocolClass: Swift.AnyClass) -> Bool
// 移除注册的实现子类
open class func unregisterClass(_ protocolClass: Swift.AnyClass)
```

## 3 Determining If a Subclass Can Handle a Request

```swift
// 是否拦截该请求，并处理
open class func canInit(with request: URLRequest) -> Bool
```

## 4 Getting and Setting Request Properties

```swift
// 通过属性key获取值
open class func property(forKey key: String, in request: URLRequest) -> Any?
// 动态添加属性可以和对应的值 
open class func setProperty(_ value: Any, forKey key: String, in request: NSMutableURLRequest)
// 移除属性key和对应的值
open class func removeProperty(forKey key: String, in request: NSMutableURLRequest)
```

## 5 Providing a Canonical Version of a Request

```swift
// 将拦截的请求转换为另一个请求处理
open class func canonicalRequest(for request: URLRequest) -> URLRequest
```

## 6 Determining If Requests Are Cache Equivalent

```swift
// 验证两个请求是否使用同样的缓存
open class func requestIsCacheEquivalent(_ a: URLRequest, to b: URLRequest) -> Bool
```

## 7 Starting and Stopping Downloads

```swift
// 开始加载数据
open func startLoading()
// 加载数据结束
open func stopLoading()
```

## 8 Getting Protocol Attributes

```swift
// 数据加载器
open var client: URLProtocolClient? { get }
// 发出的请求
open var request: URLRequest { get }
// 缓存数据
@NSCopying open var cachedResponse: CachedURLResponse? { get }
```

## 9 Initializers

```swift
// 初始化
@available(iOS 8.0, *)
public convenience init(task: URLSessionTask, cachedResponse: CachedURLResponse?, client: URLProtocolClient?)
```

## 10 Instance Properties

```swift
// 会话任务
@available(iOS 8.0, *)
@NSCopying open var task: URLSessionTask? { get }
```

## 11 Type Methods

```swift
// 是否拦截处理会话任务
@available(iOS 8.0, *)
open class func canInit(with task: URLSessionTask) -> Bool
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[URLProtocol](https://developer.apple.com/reference/foundation/urlprotocol)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-02-10 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974