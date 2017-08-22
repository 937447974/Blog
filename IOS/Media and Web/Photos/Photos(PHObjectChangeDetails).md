[Photos(PHChange)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHChange).md)

[Photos(PHObjectChangeDetails)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHObjectChangeDetails).md)

[Photos(PHFetchResultChangeDetails)](https://github.com/937447974/Blog/blob/master/IOS/Media%20Layer/Photos/Photos(PHFetchResultChangeDetails).md)

---

PHObjectChangeDetails主要用于记录PHObject子类数据的变动，即PHAsset、PHAssetCollection和PHCollectionList的变动。

# 1 获取变化对象

```swift
/// 改变前
public var objectBeforeChanges: PHObject { get }
    
/// 改变后
public var objectAfterChanges: PHObject? { get }
```

# 2 获取变化信息

```swift
// 内容是否发生变化
public var assetContentChanged: Bool { get }
    
// 该对象是否删除
public var objectWasDeleted: Bool { get }
```

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHChange Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHChange_Class/index.html)

[PHObjectChangeDetails Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHObjectChangeDetails_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-05 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog