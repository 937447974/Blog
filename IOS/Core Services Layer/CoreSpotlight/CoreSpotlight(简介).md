 使用CoreSpotligh库，可以使我们的应用在搜索栏添加检索内容，使用户快速搜索想要查询的内容，如下图所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012802.png)![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016012803.png)


#Classes

- NSObject
    - CSCustomAttributeKey
The CSCustomAttributeKey class defines a key that you can associate with a custom attribute for a searchable item.
    - CSIndexExtensionRequestHandler
The CSIndexExtensionRequestHandler class defines an interface you use to implement an index-maintenance app extension.
    - CSPerson
A CSPerson object represents a person in the context of search results.
    - CSSearchableIndex
The CSSearchableIndex class defines an object that represents the on-device index.
    - CSSearchableItem
The CSSearchableItem class defines an object that represents an item that can be indexed and made available to users when they search on their devices.
    - CSSearchableItemAttributeSet
The CSSearchableItemAttributeSet class defines an object that encapsulates the set of properties you want to display for a searchable item (that is, an item represented by a CSSearchableItem object).
- NSString
    - CSLocalizedString 描述性文字。

&#160;

----

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation

[Core Spotlight Framework Reference](https://developer.apple.com/library/ios/documentation/CoreSpotlight/Reference/CoreSpotlight_Framework/index.html)

[App Search Programming Guide](https://developer.apple.com/library/ios/documentation/General/Conceptual/AppSearch/index.html)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-28 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog