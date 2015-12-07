#1 简介

使用NSJSONSerialization，我们可以将JSON转换为AnyObject对象，或将AnyObject对象转换为JSON对象。

将一个AnyObject对象转换JSON对象必须具备以下条件。

1. 该对象的是NSArray或NSDictionary。
2. 内部数据只能是NSString, NSNumber, NSArray, NSDictionary, 或NSNull。
3. 所有字典的key是NSString。
4. Number类型数据不能是NaN或无穷。

你还可以通过方法`isValidJSONObject:`判断一个对象能否转换成JSON对象。

JSON对象其实质是一个NSDAta对象。

#2 AnyObject转JSON

```swift
var dict = [String: String]()
dict["name"] = "阳君"
dict["qq"] = "937447974"
do {
    if NSJSONSerialization.isValidJSONObject(dict) { // 能否转换为JSON Data
        // 转换为JSON Data
        let data  = try NSJSONSerialization.dataWithJSONObject(dict, options: NSJSONWritingOptions.PrettyPrinted)
        // 转换为json串
        self.jsonString = String(data: data, encoding: NSUTF8StringEncoding)
        print("json生成:\(self.jsonString)")
    }
} catch {
    print("转换出错:\(error)")
}
```

#3 JSON转AnyObject

```swift
// json转data
if let data = self.jsonString?.dataUsingEncoding(NSUTF8StringEncoding) {
    do {
        // data转JSON Object
        let jsonObject = try NSJSONSerialization.JSONObjectWithData(data, options: NSJSONReadingOptions.AllowFragments)
        // JSON Object转实际对象
        if let dict = jsonObject as? Dictionary<String, AnyObject> {
            print("json解析:\(dict)")
        }
    } catch {
        print("解析xml出错:\(error)")
    }
}
```

&#160;

----------

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[NSJSONSerialization Class Reference](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSJSONSerialization_Class/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-07 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog