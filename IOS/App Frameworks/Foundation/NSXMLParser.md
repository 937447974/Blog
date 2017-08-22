**1 [NSXMLParser](#1)**

1.1 [Initializing a Parser Object](#1.1)

1.2 [Managing Delegates](#1.2)

1.3 [Managing Parser Behavior](#1.3)

1.4 [Parsing](#1.4)

1.5 [Obtaining Parser State](#1.5)

**2 [NSXMLParserDelegate](#2)**

**3 [实战演练](#3)**

3.1 [XML文件](#3.1)

3.2 [测试](#3.2)

----

# <a id="1">1 NSXMLParser

使用NSXMLParser，我们可以解析XML文档。NSXMLParser通过委托代理的方式，将XML中的元素通过代理返回

## <a id="1.1">1.1 Initializing a Parser Object

```swift
/// 通过xml路径生成
public convenience init?(contentsOfURL url: NSURL)

/// 通过NSData生成
public init(data: NSData)

/// 通过流生成
@available(iOS 5.0, *)
public convenience init(stream: NSInputStream)
```

## <a id="1.2">1.2 Managing Delegates

```swift
/// 代理
unowned(unsafe) public var delegate: NSXMLParserDelegate?
```

## <a id="1.3">1.3 Managing Parser Behavior

```swift
/// 是否显示限定名称（parser:didStartElement:namespaceURI:qualifiedName:attributes: and parser:didEndElement:namespaceURI:qualifiedName:）
public var shouldProcessNamespaces: Bool

/// 是否显示命名空间声明（parser:didStartMappingPrefix:toURI: and parser:didEndMappingPrefix:）
public var shouldReportNamespacePrefixes: Bool

/// 是否禁用实体的切换（parser:foundExternalEntityDeclarationWithName:publicID:systemID:）
public var shouldResolveExternalEntities: Bool
```

## <a id="1.4">1.4 Parsing

```swift
/// 开始解析
public func parse() -> Bool
    
/// 停止解析
public func abortParsing()
    
/// 解析错误
@NSCopying public var parserError: NSError? { get }
```

## <a id="1.5">1.5 Obtaining Parser State

```swift
/// 外部实体的公共标识符
public var publicID: String? { get }
/// 外部系统的标识符
public var systemID: String? { get }
/// 行
public var lineNumber: Int { get }
/// 列
public var columnNumber: Int { get }
```

# <a id="2">2 NSXMLParserDelegate

关于NSXMLParserDelegate多数情况下，我们只使用下面几个协议。

```swift
/// MARK: 解析开始
optional public func parserDidStartDocument(parser: NSXMLParser)

/// MARK: 解析结束
optional public func parserDidEndDocument(parser: NSXMLParser)

/// MARK: 解析出错
optional public func parser(parser: NSXMLParser, parseErrorOccurred parseError: NSError)

/// MARK: 解析器每次在XML中找到新的元素时，会调用该方法
optional public func parser(parser: NSXMLParser, didStartElement elementName: String, namespaceURI: String?, qualifiedName qName: String?, attributes attributeDict: [String : String])
```

# <a id="3">3 实战演练

## <a id="3.1">3.1 XML文件

在项目中创建文件，文件名为Main.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<users xmlns="http://blog.csdn.net/y550918116j">
    <user id="0" name="阳君" qq="937447974"/>
    <user id="1" name="阳君" qq="937447974"/>
</users>
```

## <a id="3.2">3.2 测试

```swift
//
//  YJXMLParserVC.swift
//  YJFoundation
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by admin on 16/3/10.
//  Copyright © 2016年 阳君. All rights reserved.
//

import UIKit

/// NSXMLParser解析XML
class YJXMLParserVC: UIViewController, NSXMLParserDelegate {

    override func viewDidLoad() {
        super.viewDidLoad()
        if let url = NSBundle.mainBundle().URLForResource("Main", withExtension: "xml") {
            if let parser = NSXMLParser(contentsOfURL: url) {
                parser.shouldProcessNamespaces = false;
                parser.delegate = self;
                parser.parse()
            }
        }
    }
    
    // MARK: - NSXMLParserDelegate
    // MARK: 解析开始
    func parserDidStartDocument(parser: NSXMLParser) {
        print(__FUNCTION__)
    }
    
    // MARK: 解析结束
    func parserDidEndDocument(parser: NSXMLParser) {
        print(__FUNCTION__)
    }
    
    // MARK: 解析出错
    func parser(parser: NSXMLParser, parseErrorOccurred parseError: NSError) {
        print("解析错误\(parseError.localizedDescription)")
    }
    
    // MARK: 解析器每次在XML中找到新的元素时，会调用该方法
    func parser(parser: NSXMLParser, didStartElement elementName: String, namespaceURI: String?, qualifiedName qName: String?, attributes attributeDict: [String : String]) {
        print("\(elementName) - \(namespaceURI) - \(qName) - \(attributeDict)")
    }

}
```

&#160;

----------

# Appendix

## Sample Code

[Swift](https://github.com/937447974/Swift)

## Related Documentation

[NSXMLParser Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSXMLParser_Class/index.html)

[NSXMLParserDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/NSXMLParserDelegate_Protocol/index.html)

[Event-Driven XML Programming Guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/XMLParsing/XMLParsing.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-08 | 博文完成 |
| 2016-03-10 | 博文更新，增加NSXMLParser的相关说明 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog