# 1 WebSocket 介绍

WebSocket 是 HTML5 开始提供的一种在 TCP 上进行的套接字全双工通讯协议，可以实现客户端与服务器端的异步通信，实现服务器的推送功能。WebSocket 和 Http 的区别如下所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017102301.png)

WebSocket 是基于 TCP 通信，浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，首次的请求是使用的 HTTP 连接，连接之后浏览器和服务器之间就形成了一条快速通道。两者之间就可以通过 TCP 通道进行数据互相传送。

WebSocket 传输的数据格式比较轻量，可以发送纯文本，也可以直接发送二进制数据。和传统的 Http 长连接相比，性能开销小，通信高效。如图所示 和 http 一样它有有加密方式 ws(不加密) 和 wss(加密)。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017102302.png)

WebSocket 是 HTML5 增加的通信协议，并不是所有的浏览器都支持，相关可支持的版本如下所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017102304.png)

# 2 Spring WebSocket

在Spring 中开发 WebSocket 框架主要基于 spring-websocket 和 spring-messaging 库。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017102303.png)

在 WebSocket 中，主要包含如下信息。

1. Message消息，包含消息头和负载。消息头中包含，信息id、优先级等；负载是传输的信息，可以放任何数据。
2. Channel 是一个管道，服务器上产一个 Message 放入 Channel，消费者通过Subscribe(订阅)从 Message 消费一个 Message。Channel 上可以加设拦截器拦截非法请求。
3. EndPoint 是服务器的接入点，客户端通过这个接入点建立 WebSocket 通信。

在浏览器我们并不是直接使用原生的的 WebSocket 协议，而是使用更高级的 SockJS 和 StompJS，SockJS 是 WebSocketJS 的。StompJS 是 SockJS 的高级封装，它对集群架构有更好的支持。

## 2.1 SockJS

为了应对许多浏览器不支持 WebSocket 协议的问题，Spring 提供了备选协议 [SockJS](https://github.com/sockjs/sockjs-client/) 的支持。

SockJS 的请求格式如下：

`http://host:port/myApp/myEndpoint/{server-id}/{session-id}/{transport}`

## 2.2 Stomp

SockJS 是 WebSocket 的备选方案，但它同样是一种偏底层的协议，Spring 建议我们使用它们的高级协议 [STOMP](https://stomp.github.io/index.html)。

STOMP 是一种简单的面向文本的消息传递协议，其前身是 TTMP 协议（一个简单的基于文本的协议），专为消息中间件设计，如 ActiveMQ。它提供了一个可互操作的连接格式，允许 STOMP 客户端与任意 STOMP 消息代理（Broker）进行交互。虽然 STOMP 是一种面向文本的协议，但消息的有效负载可以是文本或二进制文件。

同 HTTP 在 TCP 套接字上添加请求-响应模型层一样，STOMP 在 WebSocket 之上提供了一个基于帧的线路格式层，用来定义消息语义。

STOMP 帧由命令，一个或多个头信息以及负载所组成。如下就是发送数据的一个 STOMP 帧：

```
SEND
destination:/queue/trade
content-type:application/json
content-length:44

{"action":"BUY","ticker":"MMM","shares",44}^@
```

# 3 实战

## 3.1 JSP页面

JS 使用 SockJS 和 Stomp 完成开发。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html lang="en">
<head>
    <title>WebSocket</title>
    <script src="${pageContext.request.contextPath}/js/jquery.js"></script>
    <script src="${pageContext.request.contextPath}/js/sockjs.js"></script>
    <script src="${pageContext.request.contextPath}/js/stomp.js"></script>
    <script type="text/javascript">
        $(document).ready(function () {
            setConnected(false);
        });

        var stompClient = null;

        // 连接
        function connect() {
            var userId = $("#userId").val();
            var socket = new SockJS("/stomp");
            stompClient = Stomp.over(socket);
            stompClient.connect({'userId': userId}, function (frame) {
                setConnected(true);
                console.log('连接成功: ' + frame);
                showGreeting("连接成功");
                // 广播
                stompClient.subscribe('/topic/message', function (greeting) {
                    console.log('subscribe: ' + greeting);
                    showGreeting(JSON.parse(greeting.body).content);
                });
                // 一对一通信
                stompClient.subscribe('/user/' + userId + '/message', function (greeting) {
                    console.log('subscribe: ' + greeting);
                    showGreeting(JSON.parse(greeting.body).content);
                });
            }, function (error) {
                console.log('connect error: ' + error);
                alert(error);
            });
        }

        function disconnect() {
            if (stompClient != null) {
                stompClient.disconnect(function () {
                    setConnected(false);
                    console.log("断开");
                });
            }
        }

        function send() {
            if (stompClient.connected) {
                var body = JSON.stringify({
                    'userId': $("#send_userId").val(),
                    'content': $("#send_content").val()
                });
                stompClient.send("/app/stomp/send", {'type': 'text'}, body);
                console.log("send: " + body);
            } else {
                setConnected(false);
            }
        }

        // 显示信息
        function showGreeting(message) {
            var response = document.getElementById('response');
            var p = document.createElement('p');
            p.style.wordWrap = 'break-word';
            p.appendChild(document.createTextNode(message));
            response.appendChild(p);
        }

        function setConnected(connected) {
            $("#connect").prop("disabled", connected);
            $("#disconnect").prop("disabled", !connected);
            if (connected) {
                $("#conversationDiv").show();
            } else {
                $("#conversationDiv").hide();
            }
            $("#response").html('');
        }
    </script>
</head>
<body>
<div>
    <div>
        <label>登录</label><br/>
        <input type="text" id="userId" placeholder="输入账号" value="937447974"/>
        <button id="connect" onclick="connect();">连接</button>
        <button id="disconnect" disabled="disabled" onclick="disconnect();">断开</button>
    </div>
    <div id="conversationDiv">
        <label>发送消息</label>
        <br/>
        <input type="text" id="send_userId" placeholder="对方账号"/>
        <input type="text" id="send_content" placeholder="输入信息"/>
        <button onclick="send();">发送</button>
        <br/>
        <label>控制台</label><br/>
        <p id="response"></p>
    </div>
</div>
</body>
</html>
```

可以看到客户端开发只需要建立连接，添加订阅，发送消息和断开连接相关功能。

## 3.2 WebSocket 配置

xml 中 配置 SpringMVC 的相关配置。WebSocket 配置使用 Java 的方式开发，这有利于模块独立化。

```java
package com.websocket.config;

/**
 * WebsocketConfig.java
 * Websocket 配置
 * Created by 阳君 on 2017/10/20.
 * Copyright © 2017年 websocket. All rights reserved.
 */
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig extends AbstractWebSocketMessageBrokerConfigurer {

    @Autowired
    private InboundChannelInterceptor inboundChannelInterceptor;

    // 连接站点配置
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/stomp") // stomp 连接点
                .addInterceptors(new AuthHandshakeInterceptor()) // 拦截器
                .setAllowedOrigins("*")
                .withSockJS();
    }

    // 消息传输参数配置
    @Override
    public void configureWebSocketTransport(WebSocketTransportRegistration registration) {
        super.configureWebSocketTransport(registration);
        registration.setSendTimeLimit(15 * 1000)    // 超时时间
                .setSendBufferSizeLimit(512 * 1024) // 缓存空间
                .setMessageSizeLimit(128 * 1024);   // 消息大小
    }

    // 输入通道配置
    @Override
    public void configureClientInboundChannel(ChannelRegistration registration) {
        super.configureClientInboundChannel(registration);
        registration.interceptors(this.inboundChannelInterceptor);// 设置拦截器
        registration.taskExecutor()    // 线程信息
                .corePoolSize(400)     // 核心线程池
                .maxPoolSize(800)      // 最多线程池数
                .keepAliveSeconds(60); // 超过核心线程数后，空闲线程超时60秒则杀死
    }

    // 输出通道配置
    @Override
    public void configureClientOutboundChannel(ChannelRegistration registration) {
        super.configureClientOutboundChannel(registration);
    }

    // 配置消息转化器
    @Override
    public boolean configureMessageConverters(List<MessageConverter> messageConverters) {
        return super.configureMessageConverters(messageConverters);
    }

    // 配置消息代理
    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        super.configureMessageBroker(registry);
        registry.enableSimpleBroker("/topic", "/user"); // 推送消息前缀
        registry.setApplicationDestinationPrefixes("/app")                // 应用请求前缀
                .setUserDestinationPrefix("/user/");                      // 推送用户前缀
    }

}
```

&#160;

## 3.3 拦截器

### 3.3.1 握手拦截

WebSocket 首次连接时，会使用 http 的连接方式，默认使用了 OriginHandshakeInterceptor 拦截，它主要对可访问的域名拦截，我们可以通过继承它做连接处理。

```java
package com.websocket.config;

import org.springframework.http.server.ServerHttpRequest;
import org.springframework.http.server.ServerHttpResponse;
import org.springframework.lang.Nullable;
import org.springframework.web.socket.WebSocketHandler;
import org.springframework.web.socket.server.support.OriginHandshakeInterceptor;

import java.util.Map;

/**
 * AuthHandshakeInterceptor.java
 * 握手拦截器
 * Created by 阳君 on 2017/10/21.
 * Copyright © 2017年 websocket. All rights reserved.
 */
public class AuthHandshakeInterceptor extends OriginHandshakeInterceptor {

    @Override
    public boolean beforeHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler
            wsHandler, Map<String, Object> attributes) throws Exception {
        System.out.println("beforeHandshake");
        return super.beforeHandshake(request, response, wsHandler, attributes);
    }


    @Override
    public void afterHandshake(ServerHttpRequest request, ServerHttpResponse response, WebSocketHandler wsHandler,
                               @Nullable Exception ex) {
        System.out.println("afterHandshake");
        super.afterHandshake(request, response, wsHandler, ex);
    }
}
```

这个类中的方法，只会在连接时执行一次，数据传输过程中会走通道拦截。

### 3.3.2 通道拦截

WebSockt 是通过 Channel 传输数据，通过继承 ChannelInterceptorAdapter 可做到对输入和输出数据的处理。

这里做了输入通道的拦截，通过拦截我们可以自定义相关命令的实现。

```java
package com.websocket.config;

import com.websocket.service.WebSocketService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.lang.Nullable;
import org.springframework.messaging.Message;
import org.springframework.messaging.MessageChannel;
import org.springframework.messaging.simp.stomp.StompCommand;
import org.springframework.messaging.simp.stomp.StompHeaderAccessor;
import org.springframework.messaging.support.ChannelInterceptorAdapter;
import org.springframework.messaging.support.MessageHeaderAccessor;
import org.springframework.stereotype.Component;

/**
 * InboundChannelInterceptor.java
 * 输入通道拦截器
 * Created by 阳君 on 2017/10/21.
 * Copyright © 2017年 websocket. All rights reserved.
 */
@Component
public class InboundChannelInterceptor extends ChannelInterceptorAdapter {

    @Autowired
    private WebSocketService webSocketService;

    @Override
    public Message<?> preSend(Message<?> message, MessageChannel channel) {
        System.out.println("preSend:" + message.getHeaders());
        StompHeaderAccessor accessor = MessageHeaderAccessor.getAccessor(message, StompHeaderAccessor.class);
        StompCommand stompCommand = accessor.getCommand();
        if (StompCommand.CONNECT.equals(stompCommand)) {
            String userId = accessor.getFirstNativeHeader("userId");
            String simpSessionId = accessor.getHeader("simpSessionId").toString();
            this.webSocketService.connect(simpSessionId, userId);
        } else if (StompCommand.DISCONNECT.equals(stompCommand)) {
            this.webSocketService.disconnect(accessor.getHeader("simpSessionId").toString());
        }
        return super.preSend(message, channel);
    }

    @Override
    public void postSend(Message<?> message, MessageChannel channel, boolean sent) {
        System.out.println("postSend输入数据处理后");
        super.postSend(message, channel, sent);
    }

    @Override
    public void afterSendCompletion(Message<?> message, MessageChannel channel, boolean sent, @Nullable Exception ex) {
        super.afterSendCompletion(message, channel, sent, ex);
        System.out.println("afterSendCompletion");
    }
}
```

&#160;

## 3.4 控制器

控制器写法和 SpringMVC，主要是使用 @MessageMapping 做匹配处理。@SendTo 和 @SendToUser 可以做返回数据处理，但实际开发中，多数会自定义返回操作。

```java
package com.websocket.controller;

import com.websocket.po.TextMessage;
import com.websocket.service.WebSocketService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.messaging.handler.annotation.Header;
import org.springframework.messaging.handler.annotation.Headers;
import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.util.StringUtils;
import org.springframework.web.bind.annotation.RestController;

import java.util.Map;

/**
 * WebSocketController.java
 * <p>
 * Created by 阳君 on 2017/10/20.
 * Copyright © 2017年 websocket. All rights reserved.
 */
@MessageMapping("/stomp")
@RestController
public class WebSocketController {
    // @SendTo("/topic/message") 广播发送给 /topic/message 订阅客户端
    // @SendToUser("1/message") 发送给指定订阅用户

    @Autowired
    public WebSocketService webSocketService;

    @MessageMapping("/send")
    public void sendBroadcast(@Headers Map<String, Object> headers, @Header String simpSessionId, TextMessage
            textMessage) {
        TextMessage sendTM = new TextMessage(this.webSocketService.getPool().get(simpSessionId) + ": " + textMessage
                .getContent());
        String userId = textMessage.getUserId();
        if (StringUtils.isEmpty(userId)) {
            this.webSocketService.send(sendTM);
        } else {
            this.webSocketService.sendToUser(userId, sendTM);
        }
    }

}
```

&#160;

## 3.5 业务层

业务层使用了 WebSocketService ，这里我们实现了登录、断开、群发和定点发送消息的吹。

```java
package com.websocket.service;

import com.websocket.po.TextMessage;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.messaging.MessagingException;
import org.springframework.messaging.simp.SimpMessagingTemplate;
import org.springframework.stereotype.Service;
import org.springframework.util.StringUtils;

import javax.annotation.PostConstruct;
import java.util.HashMap;
import java.util.Map;

/**
 * WebSocketService.java
 * 服务层
 * Created by 阳君 on 2017/10/22.
 * Copyright © 2017年 websocket. All rights reserved.
 */
@Service
public class WebSocketService {

    private Map<String, String> pool; // 连接池
    @Autowired
    private SimpMessagingTemplate simpMessagingTemplate;

    @PostConstruct
    void init() {
        this.pool = new HashMap<String, String>();
    }

    public Map<String, String> getPool() {
        return pool;
    }

    /**
     * 连接
     *
     * @param simpSessionId 会话id
     * @param userId        用户id
     */
    public void connect(String simpSessionId, String userId) {
        if (!(StringUtils.isEmpty(simpSessionId) || StringUtils.isEmpty(userId))) {
            this.pool.put(simpSessionId, userId);
            TextMessage textMessage = new TextMessage(userId + "上线，当前在线人数：" + this.pool.size());
            this.simpMessagingTemplate.convertAndSend("/topic/message", textMessage);
        }
    }


    /**
     * 断开连接
     *
     * @param simpSessionId 会话id
     */
    public void disconnect(String simpSessionId) {
        String userId = this.pool.get(simpSessionId);
        if (userId != null) {
            this.pool.remove(simpSessionId);
            TextMessage textMessage = new TextMessage(userId + "离线，当前在线人数：" + this.pool.size());
            this.send(textMessage);
        }
    }

    /**
     * 给所有用户发送消息
     *
     * @param payload 消息
     * @throws MessagingException
     */
    public void send(Object payload) throws MessagingException {
        this.simpMessagingTemplate.convertAndSend("/topic/message", payload);
    }

    /**
     * 给指定用户发消息
     *
     * @param userId  用户id
     * @param payload 消息
     * @throws MessagingException
     */
    public void sendToUser(String userId, Object payload) throws MessagingException {
        this.simpMessagingTemplate.convertAndSendToUser(userId, "/message", payload);
    }

}
```

4

## 3.6 数据模型

传输的数据模型使用 TextMessage 完成。

```java
package com.websocket.po;

import org.springframework.lang.Nullable;

public class TextMessage {

    @Nullable
    private String userId; // 用户id
    @Nullable
    private String content; // 内容

    public TextMessage() {
    }

    public TextMessage(String content) {
        this.content = content;
    }

    @Nullable
    public String getUserId() {
        return userId;
    }

    @Nullable
    public String getContent() {
        return content;
    }
}
```

为了避免客户端传空，引起解析报错，需在字段上添加 @Nullable。


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

[WebSocket+SockJs+STMOP](http://www.jianshu.com/p/4ef5004a1c81)

[STOMP协议详解](http://blog.csdn.net/chszs/article/details/46592777)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-10-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974