当我们在开发过程中对系统类扩展时，会发现无法扩展属性。如果想扩展属性，可使用Runtime扩展。

#1 相关API

其中涉及到的Runtime方法有如下三个。

```swift
/// 设置值
///
/// - parameter object : self
/// - parameter key : 属性
/// - parameter value : 值
/// - parameter policy : 存储策略objc_AssociationPolicy
///
/// - returns: void
public func objc_setAssociatedObject(object: AnyObject!, _ key: UnsafePointer<Void>, _ value: AnyObject!, _ policy: objc_AssociationPolicy)
    
    
/// 获取值
///
/// - parameter object : self
/// - parameter key : 属性
///
/// - returns: AnyObject
public func objc_getAssociatedObject(object: AnyObject!, _ key: UnsafePointer<Void>) -> AnyObject!
    
/// 去掉所有关联属性
///
/// - parameter object : self
///
/// - returns: void
public func objc_removeAssociatedObjects(object: AnyObject!)
```

#2 实战演练

##2.1 扩展属性

下面为YJUser扩展name属性。

```swift
//
//  YJUser.swift
//  Runtime
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/11.
//  Copyright © 2016年 阳君. All rights reserved.
//

import Cocoa

/// 用户
class YJUser: NSObject {
    
}

/// 扩展
extension YJUser {
    
    /// 用户名
    var name: String {
        get {
            return objc_getAssociatedObject(self, "extensionName") as! String
        }
        set {
            objc_setAssociatedObject(self, "extensionName", newValue, objc_AssociationPolicy.OBJC_ASSOCIATION_COPY)
        }
    }
    
    /// 清空所有关联属性
    ///
    /// - returns: void
    func cleanParameter() {
        // 此时再获取关联属性的值，会报错
        objc_removeAssociatedObjects(self)
    }
}
```

##2.2 测试

```swift
//
//  main.swift
//  Runtime
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/11.
//  Copyright © 2016年 阳君. All rights reserved.
//

import Foundation

let user = YJUser()
user.name = "阳君"
print(user.name)
user.cleanParameter()
print(user.name)
```


&#160;

----------

#Appendix

##Sample Code

[Runtime](https://github.com/937447974/Swift/tree/master/Runtime)

##Related Documentation

[Objective-C Runtime Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-11 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog