Spring MVC 中的拦截器类似于 Servlet 中的过滤器，它主要用于拦截用户请求并做相应的处理。

拦截器只需要在xml中如下配置即可。

```xml
<mvc:interceptors>
        <bean class="com.CustomInterceptor"/>
        <mvc:interceptor>
                <mvc:mapping path="/**"/>
                <mvc:exclude-mapping path="/secure/**"/>
                <bean class="com.Interceptor1"/>
        </mvc:interceptor>
        <mvc:interceptor>
                <mvc:mapping path="/secure/*"/>
                <bean class="com.Interceptor2"/>
        </mvc:interceptor>
</mvc:interceptors>
```

按顺序拦截，会经过 CustomInterceptor、Interceptor1 和 Interceptor2。

拦截器主要实现 HandlerInterceptor 接口，接口定义如下

```java
package org.springframework.web.servlet;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.lang.Nullable;
import org.springframework.web.method.HandlerMethod;

public interface HandlerInterceptor {

    // 控制器方法前执行，其返回值表示是否中断后续操作
    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws
            Exception {
        return true;
    }

    // 控制器方法调用之后，解析视图之前执行
    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable
            ModelAndView modelAndView) throws Exception {
    }

    // 整个请求完成，视图渲染结束之后执行
    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
                                 @Nullable Exception ex) throws Exception {
    }

}
```

多个拦截器，如 Interceptor1 和 Interceptor2，它们的核心方法执行顺序如下。

```java
Interceptor1 preHandle
Interceptor2 preHandle

HandlerAdapter handle

Interceptor2 postHandle
Interceptor1 postHandle

DispatcherServlet render

Interceptor2 afterCompletion
Interceptor1 afterCompletion
```

&#160;

----------

# Appendix

## Sample Code

[Java](https://github.com/937447974/Java)

## Related Documentation

[Spring Framework Reference](https://docs.spring.io/spring/docs/5.0.0.BUILD-SNAPSHOT/spring-framework-reference/web.html#mvc-config)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-10-19 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974