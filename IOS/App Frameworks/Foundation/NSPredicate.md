NSPredicate谓词用于快速过滤数据。

初始化的方式如下所示

```objc
NSString *attributeName  = @"firstName";
NSString *attributeValue = @"Adam";
NSPredicate *predicate   = [NSPredicate predicateWithFormat:@"%K like %@",
        attributeName, attributeValue];
```

## 1 Parser Basics

1. %@是对值为字符串，数字或者日期的对象的替换值。
2. %K是key path的替换值。

## 2 Basic Comparisons

1. =, ==：左边的表达式和右边的表达式相等。
2. >=, =>：左边的表达式大于或者等于右边的表达式。
3. <=, =<：左边的表达式小于等于右边的表达式。
4. >：左边的表达式大于右边的表达式。
5. <：左边的表达式小于右边的表达式。
6. !=, <>：左边的表达式不等于右边的表达式。
7. BETWEEN：左边的表达式等于右边的表达式的值或者介于它们之间。右边是一个有两个指定上限和下限的数值的数列（指定顺序的数列）。比如，1 BETWEEN { 0 , 33 }，或者$INPUT BETWEEN { $LOWER, $UPPER }。

## 3 Boolean Value Predicates

1. TRUEPREDICATE：总是评估为真。
2. FALSEPREDICATE：总是评估为假。

## 4 Basic Compound Predicates

1. AND, &&：逻辑与。
2. OR, ||：逻辑或。
3. NOT, !：逻辑非。

## 5 String Comparisons

字符串比较在默认的情况下是区分大小写和音调的。你可以在方括号中用关键字符c和d来修改操作符以相应的指定不区分大小写和变音符号，比如firstname BEGINSWITH[cd] $FIRST_NAME。

1. BEGINSWITH：左边的表达式以右边的表达式作为开始。
2. CONTAINS：左边的表达式包含右边的表达式。
3. ENDSWITH：左边的表达式以右边的表达式作为结束。
4. LIKE：左边的表达式等于右边的表达式：?和*可作为通配符，其中?匹配1个字符，*匹配0个或者多个字符。
5. MATCHES：左边的表达式根据ICU v3（更多内容请查看[ICU User Guide for Regular Expressions](http://userguide.icu-project.org/strings/regexp)）的regex风格比较，等于右边的表达式。

## 6 Aggregate Operations

1. ANY，SOME：指定下列表达式中的任意元素。比如，ANY children.age < 18。
2. ALL：指定下列表达式中的所有元素。比如，ALL children.age < 18。
3. NONE：指定下列表达式中没有的元素。比如，NONE children.age < 18。它在逻辑上等于NOT (ANY ...)。
4. IN：等于SQL的IN操作，左边的表达必须出现在右边指定的集合中。比如，name IN { 'Ben', 'Melissa', 'Nick' }。
5. array[index]：指定数组中特定索引处的元素。
6. array[FIRST]：指定数组中的第一个元素。
7. array[LAST]：指定数组中的最后一个元素。
8. array[SIZE]：指定数组的大小。

&#160;

----------

# Appendix

## Related Documentation

[Predicate Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Predicates/AdditionalChapters/Introduction.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-10-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974