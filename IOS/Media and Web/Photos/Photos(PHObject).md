Photos是一个基类，其子类有

- PHAsset：照片或视频，包含ICloud的照片或视频。
- PHCollection：抽象类，定义了共享的照片。
    - PHAssetCollection：照片或视频集合（照片App中的相薄）。
    - PHCollectionList：照片或集合的分组（照片App中的照片）。
- PHObjectPlaceholder：只读代理，代表对象未被创建。

在其内部有一个属性。

```swift
/// 是继承Photos对象的唯一标示符
var localIdentifier: String { get }
```

当你想直接查询某张照片、相册或集合时，可使用这个属性查询。

&#160;

----------

# 其他

## 源代码

[Swift](https://github.com/937447974/Swift)

## 参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

[PHObject Class Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/PHObject_Class/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-05 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog