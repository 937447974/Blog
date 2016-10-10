#1 PHPhotoLibrary
 
PHPhotoLibrary是Photos库最核心的类。它是我们的APP沟通用户照片库的桥梁。我们可以通过它增加、删除、修改用户的照片库。

##1.1 身份验证

如大多数的系统功能，如推送。这些都是用户的隐私功能，需要用户授权才可以使用的。Photos库使用的也是用户的隐私数据。故需要用户授权，下面两个方法可以取得用户的权限及让用户授权。

```swift
// 获取用户授权
public class func authorizationStatus() -> PHAuthorizationStatus

// 通知用户授权，首次授权时可使用
public class func requestAuthorization(handler: (PHAuthorizationStatus) -> Void)
```

##1.2 获取共享库

```swift
// 获取用户的照片库
public class func sharedPhotoLibrary() -> PHPhotoLibrary
```

##1.3 修改照片库

```swift
// 异步修改照片库中数据
public func performChanges(changeBlock: dispatch_block_t, completionHandler: ((Bool, NSError?) -> Void)?)

// 同步修改照片库中数据
public func performChangesAndWait(changeBlock: dispatch_block_t) throws
```

##1.4 照片库变化通知

你可以在你的app中时刻监听照片库的状态

```swift
// 注册通知
public func registerChangeObserver(observer: PHPhotoLibraryChangeObserver)

// 注销通知
public func unregisterChangeObserver(observer: PHPhotoLibraryChangeObserver)
```

##1.5 常量

用户的授权状态是一个枚举PHAuthorizationStatus。

```swift
public enum PHAuthorizationStatus : Int {
    case NotDetermined // 用户暂未权限认证
    case Restricted // APP禁止使用相册权限认证
    case Denied // 用户拒绝使用相册
    case Authorized // 用户允许使用相册
}
```

#2 用户授权

由于整个用户都是关于照片的操作，故我们可以在AppDelegate.swift授权。修改其`func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool`方法。

```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    // Override point for customization after application launch.
    print(__FUNCTION__)
    switch PHPhotoLibrary.authorizationStatus() {
    case PHAuthorizationStatus.NotDetermined: // 用户暂未权限认证
        print("PHAuthorizationStatus.NotDetermined")
        // 权限认证
        PHPhotoLibrary.requestAuthorization { (status:PHAuthorizationStatus) -> Void in
            print(status)
        }
    case PHAuthorizationStatus.Restricted: // APP禁止使用相册权限认证
        print("PHAuthorizationStatus.Restricted")
    case PHAuthorizationStatus.Denied: // 用户拒绝使用相册
        print("PHAuthorizationStatus.Denied")
        print("请进入 设置 -> 隐私 -> 相册 开启权限")
        // 设置-隐私-相册
    case PHAuthorizationStatus.Authorized: // 用户允许使用相册
        print("PHAuthorizationStatus.Authorized")
    }
    return true
}
```

运行项目即可看见苹果系统的提示，需要你给APP授权。

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-09 | 项目启动 |
| 2015-12-10 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog