ImageIO框架基于C语言读写UIImage，同时也支持颜色管理和访问图像元数据。

下面为大家展示获取照片缩略图的源代码代码。

```swift
//
//  YJUtilsImageIO.swift
//  YJImageIO
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 16/1/14.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit
import ImageIO

/// ImageIO库工具类
public struct YJUtilsImageIO {
    
    // MARK: - 
    /// 获取UIImage的缩略图
    static func createThumbnailImageFromImage(image: UIImage) -> UIImage? {
        guard let data = UIImagePNGRepresentation(image) else {
            return nil
        }
        guard let cgImage = YJUtilsImageIO.createThumbnailImageFromData(data, imageSize: data.length) else {
            return nil
        }
        return UIImage(CGImage: cgImage)
    }
    
    /// 获取UIImage的缩略图
    static func createThumbnailImageFromData(data: NSData, var imageSize: Int) -> CGImageRef? {
        // Create an image source from NSData; no options.
        // Make sure the image source exists before continuing.
        guard let imageSource = CGImageSourceCreateWithData(data as CFDataRef, nil) else {
            print("Image source is NULL.")
            return nil
        }
        // Package the integer as a  CFNumber object. Using CFTypes allows you to more easily create the options dictionary later.
        let thumbnailSize: CFNumberRef = CFNumberCreate(kCFAllocatorDefault, CFNumberType.IntType, &imageSize)
        // Set up the thumbnail options.
        let keys: [CFStringRef] = [kCGImageSourceCreateThumbnailWithTransform, kCGImageSourceCreateThumbnailFromImageIfAbsent, kCGImageSourceThumbnailMaxPixelSize]
        let values:[CFTypeRef] = [kCFBooleanTrue, kCFBooleanTrue , thumbnailSize]
        var kcBacks = kCFTypeDictionaryKeyCallBacks
        var vcBacks = kCFTypeDictionaryValueCallBacks
        let options: CFDictionaryRef = CFDictionaryCreate(kCFAllocatorDefault, UnsafeMutablePointer(UnsafePointer<Void>(keys)), UnsafeMutablePointer(UnsafePointer<Void>(values)), 2, &kcBacks, &vcBacks)
        // Create the thumbnail image using the specified options.
        let thumbnailImage = CGImageSourceCreateThumbnailAtIndex(imageSource, 0, options)
        // Release the options dictionary and the image source, when you no longer need them.
        /* swift 废弃
        CFRelease(thumbnailSize)
        CFRelease(options)
        CFRelease(imageSource)
        */
        // Make sure the thumbnail image exists before continuing.
        guard thumbnailImage != nil else {
            print("Thumbnail image not created from image source.")
            return nil
        }
        return thumbnailImage
    }
    
}
```

你还可以使用swift的高级封装实现，可参考如下方法。


```swift
// return image as PNG. May return nil if image has no CGImageRef or invalid bitmap format
public func UIImagePNGRepresentation(image: UIImage) -> NSData? 

// return image as JPEG. May return nil if image has no CGImageRef or invalid bitmap format. compression is 0(most)..1(least)
public func UIImageJPEGRepresentation(image: UIImage, _ compressionQuality: CGFloat) -> NSData? 

```

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Image I/O Programming Guide](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Conceptual/ImageIOGuide/imageio_intro/ikpg_intro.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-13 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog