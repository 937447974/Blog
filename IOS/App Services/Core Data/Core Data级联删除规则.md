在配置关系的时候，一定要注意Delete Rule(Delete规则)。当我们删除某个对象时，该规则决定了与之相关的那些对象应该如何处理。可供选项的规则有如下几种：

1. Nullify：大多数情况都可以采用这种某人的Delete规则。如果删除了某个对象，而该对象与与其他对象的“关系”又受制于Nullify规则，那么这些对象就会把指向该对象的“关系”清空。
2. Cascade：这种Delete规则会沿着关系来传播删除操作。
3. Deny：如果尚有其他对象与某对象相关联，那么这种Delete规则会阻止开发者删除该对象。
4. No Action：会导致对象图处于不一致状态。假如运用了这条Delete规则，那么在删除某个对象之后，开发者必须手动设定反向的关系，以确保它们都指向有效的对象。

&#160;

----------

#Appendix

##Related Documentation

[Core Data Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CoreData/index.html#//apple_ref/doc/uid/TP40001075)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-31 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974