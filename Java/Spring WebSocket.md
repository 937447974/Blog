# 1 介绍

WebSocket 是 HTML5开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

在 WebSocket API 中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。

浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。

WebSocket 和 http 的

在HTML5规范中，我最喜欢的Web技术就是正迅速变得流行的WebSocket API。WebSocket提供了一个受欢迎的技术，以替代我们过去几年一直在用的Ajax技术。这个新的API提供了一个方法，从客户端使用简单的语法有效地推动消息到服务器。让我们看一看HTML5的WebSocket API：它可用于客户端、服务器端。而且有一个优秀的第三方API，名为Socket.IO。

一、什么是WebSocket API?

WebSocket API是下一代客户端-服务器的异步通信方法。该通信取代了单个的TCP套接字，使用ws或wss协议，可用于任意的客户端和服务器程序。WebSocket目前由W3C进行标准化。WebSocket已经受到Firefox 4、Chrome 4、Opera 10.70以及Safari 5等浏览器的支持。



当你获取 Web Socket 连接后，你可以通过 send() 方法来向服务器发送数据，并通过 onmessage 事件来接收服务器返回的数据。
以下 API 用于创建 WebSocket 对象。


![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

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

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-10-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974