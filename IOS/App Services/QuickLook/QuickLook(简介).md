QuickLook库可以让我们的App在iPhone/iPad中直接预览各个文件了。官方的开发文档中说明其支持的文件类型有：

1. iWork documents
2. Microsoft Office documents (Office ‘97 and newer)
3. Rich Text Format (RTF) documents
4. PDF files
5. Images
6. Text files whose uniform type identifier (UTI) conforms to the public.text type (see Uniform Type Identifiers Reference)
7. Comma-separated value (csv) files

如果你想让用户选择一个应用程序打开一个预览项目，可以使用UIDocumentInteractionController。

# Classes

- UIViewController
    - QLPreviewController 预览文件的UIViewController。

# Protocols

- QLPreviewControllerDataSource 数据源操作。
- QLPreviewControllerDelegate 过渡动画、能否打开链接管理和QLPreviewController关闭。
- QLPreviewItem 文件Item的协议，需要自定义文件类。

&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Quick Look Framework Reference for iOS](https://developer.apple.com/library/ios/documentation/QuickLook/Reference/QuickLookFrameworkReference_iPhoneOS/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-24 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog