#1 NSXMLParser

使用NSXMLParser，我们可以解析XML文档。NSXMLParser通过委托代理的方式，将XML中的元素通过代理返回。

##1.1 初始化NSXMLParser对象

```swift
// 通过xml路径生成
public convenience init?(contentsOfURL url: NSURL)

// 通过NSData生成
public init(data: NSData)

// 通过流生成
public convenience init(stream: NSInputStream) 
```

##1.2 代理

```swift
unowned(unsafe) public var delegate: NSXMLParserDelegate?
```

##1.3 解析

```swift
// 开始解析
public func parse() -> Bool

// 停止解析
public func abortParsing()

// 解析错误
@NSCopying public var parserError: NSError? { get }
```

#2 NSXMLParserDelegate

关于NSXMLParserDelegate多数情况下，我们只使用下面几个协议。

```swift
// MARK: 解析开始
optional public func parserDidStartDocument(parser: NSXMLParser)

// MARK: 解析结束
optional public func parserDidEndDocument(parser: NSXMLParser)

// MARK: 解析出错
optional public func parser(parser: NSXMLParser, parseErrorOccurred parseError: NSError)

// MARK: 解析器每次在XML中找到新的元素时，会调用该方法
optional public func parser(parser: NSXMLParser, didStartElement elementName: String, namespaceURI: String?, qualifiedName qName: String?, attributes attributeDict: [String : String])
```

#3 实战演练

##3.1 XML文件

在项目中创建文件，文件名为Main.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<users>
    <user id="0" name="阳君" qq="937447974"/>
    <users>
        <user id="01" name="阳君" qq="937447974"/>
        <user id="02" name="阳君" qq="937447974"/>
    </users>
    <user id="1" name="阳君" qq="937447974"/>
    <user id="2" name="阳君" qq="937447974"/>
</users>
```

##3.2 XCTest测试

```swift
//
//  YJXMLParserTests.swift
//  Parser
//
//  CSDN:http://blog.csdn.net/y550918116j
//  GitHub:https://github.com/937447974/Blog
//
//  Created by yangjun on 15/12/7.
//  Copyright © 2015年 阳君. All rights reserved.
//

import XCTest

/// xml解析
class YJXMLParserTests: XCTestCase, NSXMLParserDelegate {
    
    override func setUp() {
        super.setUp()
        // Put setup code here. This method is called before the invocation of each test method in the class.
    }
    
    override func tearDown() {
        // Put teardown code here. This method is called after the invocation of each test method in the class.
        super.tearDown()
    }
    
    func testExample() {
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

#其他

##源代码

[Swift](https://github.com/937447974/Swift)

##参考资料

[NSXMLParser Class Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSXMLParser_Class/index.html)

[NSXMLParserDelegate Protocol Reference](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/NSXMLParserDelegate_Protocol/index.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-12-08 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog