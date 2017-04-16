在IOS和OSX中，Photos framework提供应用简历照片编辑的相关扩展。在IOS中，Photos framework还提供了直接访问照片和视频的通道，同时支持ICloud照片库。使用这个框架可以来检索和回放照片、编辑照片和视频内容，如相册、瞬间记录和ICloud共享相册。

# 1 产品特点和概念

在IOS中，Photos framework用于处理用户的照片库，有如下几点。

## 1.1 获取请求实体和改变

在照片框架中有如下几种实体。

1. PHAsset：图片或视频。
2. PHAssetCollection：相册或时刻记录。
3. PHCollectionList：文件夹集合。

可以通过不同的实体执行不同的工作，然后通过这些对象来获取需要的数据。

## 1.2 变化观察

使用共享的PHPhotoLibrary注册变化通知。这样当用户的照片库元数据发生改变时，照片库则会通知你，用户的照片库已改变。通知使用了PHChange类，这个提供了照片对象的相关信息。

## 1.3 支持照片应用的相关功能

使用PHCollectionList在照片应用中查询关于时刻的照片集合；使用PHAsset处理照片或视频。当ICloud照片库开启时，你可以通过所有的设备获得照片和视频。

## 1.4 加载或缓存资产和缩略图

使用PHImageManager获取照片的size或相关视频资产；该照片框架会自动下载和生成照片，并缓存这些照片。还可以使用PHCachingImageManager实现预加载。

## 1.5 照片修改

PHAsset和PHAssetChangeRequest记录照片和视频的修改记录，并将他们提交到照片库中。并支持在不同的app中修改同样的照片。并使用 PHAdjustmentData记录照片的时间。当你修改照片后，你还可以使用

# 2 类结构

- PHAdjustmentData：当用户修改照片或视频后，Photos会使用PHAdjustmentData记录照片的修改时间。
- PHAssetChangeRequest：PHPhotoLibrary闭包中创建、删除或修改PHAsset对象。
    - PHAssetCreationRequest：PHPhotoLibrary闭包中将一个新照片或视频添加到照片库中。
- PHAssetCollectionChangeRequest：PHPhotoLibrary闭包中创建、删除和修改PHAssetCollection对象。
- PHAssetResource：PHAssetResource是PHAsset对象关联的基础数据。
- PHAssetResourceCreationOptions：使用PHAssetResourceCreationOptions描述PHAssetCollection对象。
- PHAssetResourceManager：照片底层数据管理。
- PHAssetResourceRequestOptions：使用PHAssetResourceManager描述PHAssetResourceManager。
- PHChange：当资产和集合发生改变的通知对象。
- PHCollectionListChangeRequest：PHPhotoLibrary闭包中创建、删除或修改PHCollectionList对象。
- PHContentEditingInput：输入PHAsset的修改。
- PHContentEditingInputRequestOptions：使用PHContentEditingInputRequestOptions描述PHContentEditingInput。
- PHContentEditingOutput：读取PHAsset的修改。
- PHFetchOptions：检索PHAsset, PHCollection, PHAssetCollection和PHCollectionList。
- PHFetchResult：检索的结果集。
- PHFetchResultChangeDetails：记录两次相同检索的差异。
- PHImageManager：加载PHAsset的管理器。
    - PHCachingImageManager：缓存PHAsset的管理器。
- PHImageRequestOptions：使用PHImageRequestOptions描述PHImageManager
- PHLivePhoto：获取精彩瞬间。
- PHLivePhotoRequestOptions：使用PHLivePhotoRequestOptions描述PHLivePhoto
- PHObject：照片实体的基类。
    - PHAsset：照片或视频，包含ICloud的照片或视频。
    - PHCollection：抽象类，定义了共享的照片。
        - PHAssetCollection：照片或视频集合（照片App中的相薄）。
        - PHCollectionList：照片或集合的分组（照片App中的照片）。
    - PHObjectPlaceholder：只读代理，代表对象未被创建。
- PHObjectChangeDetails：记录PHObject变化状态。
- PHPhotoLibrary：共享的照片库
- PHVideoRequestOptions：使用PHVideoRequestOptions描述从PHImageManager获得的视频。

# 3 协议

PHPhotoLibraryChangeObserver：当照片库中有变化时，会通过这个协议通知APP。
&#160;

----------

# 其他

## 参考资料

[Photos Framework Reference](https://developer.apple.com/library/ios/documentation/Photos/Reference/Photos_Framework/index.html)

## 文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-09 | 博文完成 |

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog