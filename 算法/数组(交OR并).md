在工作或者面试的时候，往往会遇到对一些数组求交集或者并集的情况，本日志描述了最快求交集或者并集，并且去掉了重复数据的方法。针对那些影响效率的算法，这里不在描述。

数组求教和求并都使用Set这个集合。它能够更快查询是否存在这个数据。

```swift
//
//  ArrayIntersectUnion.swift
//  ArrayIntersectUnion
//
//  Created by yangjun on 15/11/25.
//  Copyright © 2015年 阳君. All rights reserved.
//

import Cocoa

class ArrayIntersectUnion {

    
    // MARK: 求交
    /// 两数组求交,并去掉重复数据
    ///
    /// - parameter array1 : 数组1
    /// - parameter array2 : 数组2
    ///
    /// - returns: array
    func intersect(array1: Array<String>, _ array2: Array<String>) -> Array<String> {
        var list = Array<String>()
        var set = Set<String>()
        // 存入数据
        for item in array1 {
            set.insert(item)
        }
        // 判断是否共同数据
        for item in array2 {
            if set.contains(item) {
                list.append(item)
            }
        }
        return list
    }
    
    // MARK: 求并
    /// 两数组求并
    ///
    /// - parameter array1 : 数组1
    /// - parameter array2 : 数组2
    ///
    /// - returns: array
    func union(array1: Array<String>, _ array2: Array<String>) -> Array<String> {
        var list = Array<String>()
        var set = Set<String>()
        // 添加数组1
        for item in array1 {
            if !set.contains(item) { // 已添加数据，不在重复添加
                set.insert(item)
                list.append(item)
            }
        }
        // 添加数组2
        for item in array2 {
            if !set.contains(item) {
                set.insert(item)
                list.append(item)
            }
        }
        return list
    }
    
}
```

&#160;

----------

#其他

##源代码

[Algorithms](https://github.com/937447974/Algorithms)

##参考资料

[算法导论](https://github.com/937447974/LearningMaterials)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-16 | 算法项目完成、博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog