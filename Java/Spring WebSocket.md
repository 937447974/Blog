# 1 WebSocket 介绍

WebSocket 是 HTML5 开始提供的一种在 TCP 上进行的全双工通讯协议，可以实现客户端与服务器端的通信，实现服务器的推送功能。WebSocket 和 Http 的区别如下所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017102301.png)

WebSocket 是基于 TCP 通信，浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，首次的请求是使用的 HTTP 连接，连接之后浏览器和服务器之间就形成了一条快速通道。两者之间就可以通过 TCP 通道进行数据互相传送。

WebSocket 传输的数据格式比较轻量，可以发送纯文本，也可以直接发送二进制数据。和传统的 Http 长连接相比，性能开销小，通信高效。如图所示 和 http 一样它有有加密方式 ws(不加密) 和 wss(加密)。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017102302.png)

# 2 Spring WebSocket

在Spring 中开发 WebSocket 框架主要基于 spring-websocket 和 spring-messaging 库。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017102303.png)

1. 客户端

浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。

&#160;

----------

# Appendix

## Sample Code

[Java](https://github.com/937447974/Java)

## Related Documentation

[Servlet-based WebSocket Support](https://docs.spring.io/spring/docs/5.0.0.RELEASE/spring-framework-reference/web.html#websocket)

[Spring Integeration学习](http://blog.csdn.net/hs6823/article/details/17999989)

[理解JMS规范中的持久订阅和非持久订阅](http://blog.csdn.net/aitangyong/article/details/26013387)

[WebSocket应用安全问题分析](https://security.tencent.com/index.php/blog/msg/119)

[WebSocket 教程](http://www.ruanyifeng.com/blog/2017/05/websocket.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-10-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974