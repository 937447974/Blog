[UICollectionViewDataSource](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDataSource.md)

[UICollectionViewDelegate](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegate.md)

[UICollectionViewDelegateFlowLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewDelegateFlowLayout.md)

[UICollectionViewCell](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewCell.md)

[UICollectionViewLayout](https://github.com/937447974/Blog/blob/master/IOS/Cocoa%20Touch%20Layer/UIKit/UICollectionViewLayout.md)

---

#1 UICollectionViewDataSource

UICollectionViewDataSource是UICollectionView的数据源协议，管理UICollectionView显示用的相关视图配置。

##1.1 获取Item和Section数量

```swift
// MARK: 分组
optional func numberOfSectionsInCollectionView(collectionView: UICollectionView) -> Int
    
// MARK: 每一组有多少个cell
func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int
```

##1.2 获取显示的视图

```swift
// MARK: 生成Cell
func collectionView(_ collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell

// MARK: 生成Header或Footer
optional func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, atIndexPath indexPath: NSIndexPath) -> UICollectionReusableView
```

##1.3 移动Item

```swift
// MARK: 能否移动
optional func collectionView(_ collectionView: UICollectionView, canMoveItemAtIndexPath indexPath: NSIndexPath) -> Bool

// MARK: 移动cell
collectionView(_:moveItemAtIndexPath:toIndexPath:)
```

#2 实战演练

##2.1 项目搭建

这里使用storyboard搭建项目。


UICollectionView.storyboard

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[NSURLSession](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSURLSession_class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-03 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog