# 1 简介

AOP 的全称是 Aspect-Oriented Programming，即面向切面编程。AOP 的使用，使开发人员在编写业务逻辑时开源专心于核心业务，而不用过多地关注于其他业务逻辑的实现，这不但提高了开发效率，而且增强了代码的可维护性。

目前最流行的 AOP 框架就是 AspectJ，尤其是它的注解开发，更能大大提高我们的开发效率。从 Spring 2.0 开始，Spring AOP 引入了对 AspectJ 的支持，AspectJ 扩展了 Java 语言，提供了一个专门的编译器，在编译时提供横向代码的织入。

在 AOP 中，我们会经常听到如下专业术语。

1. Aspect（切面)：在实际应用中，切面通常是指封装的用于横向插入系统功能的类。
2. Join point（连接点)：在程序执行过程中的某个阶段点，它实际是对象的一个操作。在 Spring 中，它指调用的方法。
3. Pointcut(切入点)：切面与程序流程的交叉点，即那些需要处理的连接点。
4. Advice（通知/增强处理）：AOP 框架在特定的切入点执行的增强处理，即在定义好的切入点所要执行的程序代码。
5. Target Object（目标对象）：所有被通知的对象，也称为被增强对象。
6. AOP Proxy(代理)：将通知应用到目标对象之后，被动态创建的对象。
7. Weaving(织入)：将切面代码插入到目标对象之后，被动态创建的对象。

# 2 动态代理

## 2.1 JDK 动态代理

JDK 动态代理是通过 java.lang.reflect.Proxy 实现的，调用 newProxyInstance() 方法创建代理对象。

定义一个 dao 文件。

```java
public interface UserDao {
    void addUser();
}

@Repository
public class UserDaoImpl implements UserDao {
    public void addUser() {
        System.out.println("addUser");
    }
}
```

创建切面类 MySpect，定义前置和后置方法。

```java
public class MyAspect {
    public void before() {
        System.out.println("MyAspect.before");
    }

    void after() {
        System.out.println("MyAspect.after");
    }
}
```

创建代理类 JDKProxy，实现 InvocationHandler 接口。

```java
public class JDKProxy implements InvocationHandler {

    private Object target;// 目标类

    // 创建代理方法
    public Object createProxy(Object target) {
        this.target = target;
        ClassLoader classLoader = JDKProxy.class.getClassLoader();
        Class[] interfaces = target.getClass().getInterfaces();
        return Proxy.newProxyInstance(classLoader, interfaces, this);
    }

    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // 切面
        MyAspect myAspect = new MyAspect();
        myAspect.before();
        Object object = method.invoke(this.target, args);
        myAspect.after();
        return object;
    }
}
```

使用 junit 编写测试代码。

```java
@Test
public void jdkTest() {
    JDKProxy jdkProxy = new JDKProxy();
    UserDao userDao = (UserDao) jdkProxy.createProxy(new UserDaoImpl());
    userDao.addUser();
}
```

运行可在控制台看见如下打印。

```
MyAspect.before
addUser
MyAspect.after
```

## 2.2 CGLIB 代理

JDK 动态代理有一定的局限性，它动态代理的对象必须实现一个接口。如果要对没有实现接口的类进行代理，可以使用 CGLIB 代理。

CGLIB 是一个高性能开源的代码生成包，它采用非常底层的字节码技术，对指定的目标类生成一个子类，并对子类进行增强。

创建代理类 CglibProxy，实现 org.springframework.cglib.proxy.MethodInterceptor 接口。

```java
public class CglibProxy implements MethodInterceptor {

    // 创建代理方法
    public Object createProxy(Class targetClass) {
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(targetClass);
        enhancer.setCallback(this);
        return enhancer.create();
    }

    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        MyAspect myAspect = new MyAspect();
        myAspect.before();
        Object object = methodProxy.invokeSuper(o, objects);
        myAspect.after();
        return object;
    }
}
```

使用 junit 编写测试代码。

```java
@Test
public void cglibTest() {
    CglibProxy cglibProxy = new CglibProxy();
    UserDao userDao = (UserDao) cglibProxy.createProxy(UserDaoImpl.class);
    userDao.addUser();
}
```

运行可在控制台看见如下打印。

```
MyAspect.before
addUser
MyAspect.after
```

# 3 AspectJ

AspectJ 是一个基于 Java 语言的 AOP 框架，它提供了强大的 AOP 功能。Spring AOP 引入了对 AspectJ，并允许直接使用 AspectJ 进行编程。使用 AspectJ 实现 AOP 有两种方式，xml 和注解模式，工作中基本上都是使用注解开发。

AspectJ 的注解及其描述如下所示。

| 注解名称 | 描述 |
| --- | --- |
| @Aspect | 定义一个切面 |
| @Pointcut | 定义切入点表达式，在使用时是定义了一个返回为空的空方法 |
| @Around | 环绕通知，相当于 MethodInterceptor |
| @Before | 前置通知，方法调用前执行 |
| @AfterThrowing | 异常通知，方法调用出错时调用 |
| @After | 后置 final 通知，方法调用后执行,不管是否异常该通知都会执行 |
| @AfterReturning | 后置通知，返回结果后通知 |

@Pointcut 切入点使用如下匹配符号，并支持 && 和 || 。

1. execution：用于匹配方法执行的连接点；
2. within：用于匹配指定类型内的方法执行；
3. this：用于匹配当前AOP代理对象类型的执行方法；注意是AOP代理对象的类型匹配，这样就可能包括引入接口也类型匹配；
4. target：用于匹配当前目标对象类型的执行方法；注意是目标对象的类型匹配，这样就不包括引入接口也类型匹配；
5. args：用于匹配当前执行的方法传入的参数为指定类型的执行方法；
6. @target：用于匹配当前目标对象类型的执行方法，其中目标对象持有指定的注解；
7. @args：用于匹配当前执行的方法传入的参数持有指定注解的执行；
8. @within：用于匹配所以持有指定注解类型内的方法；
9. @annotation：用于匹配当前执行方法持有指定注解的方法；

其中 execution 的使用格式如下所示

```java
execution(modifiers-pattern? ret-type-pattern declaring-type-pattern?name-pattern(param-pattern) hrows-pattern?)
```

相关例子如下所示

```java
execution(public * *(..)) // .. 代表任何参数
execution(* set*(..))
execution(* com.xyz.service.AccountService.*(..))
within(com.xyz.service.*)
within(com.xyz.service..*)// ..代表任何子包
```

使用 AspectJ 是需要导入 spring-aspects 包。

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>${spring.version}</version>
</dependency>
```

在 xml 添加如下配置。

```xml
<!--切面-->
<aop:aspectj-autoproxy/>
<!--扫描包-->
<context:component-scan base-package="com.springmvc"/>
```

切面类如下

```java
package com.springmvc.aspect;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

import java.lang.annotation.Target;

@Aspect
@Component
public class AOPAspect {

//    @Pointcut("execution(* com.springmvc.dao.*.* (..))")
//    @Pointcut("within(com.springmvc.dao.*)")
//    @Pointcut("execution(void addUser())")
    @Pointcut("this(com.springmvc.dao.UserDao)")
    private void pointCut() {
    }

    // 环绕通知
    @Around("pointCut()")
    public Object myAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        // 开始
        System.out.println("环绕开始：执行目标方法之前，模拟开启事务...");
        // 执行当前目标方法
        Object obj = proceedingJoinPoint.proceed();
        // 结束
        System.out.println("环绕结束：执行目标方法之后，模拟关闭事务...");
        return obj;
    }

    // 前置通知
    @Before("pointCut()")
    public void myBefore(JoinPoint joinPoint) {
        System.out.print("前置通知 ：模拟执行权限检查...,");
        System.out.print("目标类是：" + joinPoint.getTarget());
        System.out.println(",被织入增强处理的目标方法为：" + joinPoint.getSignature().getName());
    }

    // 异常通知
    @AfterThrowing(value = "pointCut()", throwing = "e")
    public void myAfterThrowing(JoinPoint joinPoint, Throwable e) {
        System.out.println("异常通知：出错了" + e.getMessage());
    }

    // 后置 final 通知
    @After("pointCut()")
    public void myAfter() {
        System.out.println("后置 final 通知：模拟方法结束后的释放资源...");
    }

    // 后置通知
    @AfterReturning(value = "pointCut()")
    public void myAfterReturning(JoinPoint joinPoint) {
        System.out.print("后置通知：模拟记录日志...,");
        System.out.println("被织入增强处理的目标方法为：" + joinPoint.getSignature().getName());
    }

}
```

使用 junit 测试

```java
@Test
public void springTest() {
    ApplicationContext act = new ClassPathXmlApplicationContext("applicationContext.xml");
    UserDao userDao = act.getBean(UserDao.class);
    userDao.addUser();
}
```

运行可在控制台看见如下打印

```
环绕开始：执行目标方法之前，模拟开启事务...
前置通知 ：模拟执行权限检查...,目标类是：com.springmvc.dao.UserDaoImpl@2eae8e6e,被织入增强处理的目标方法为：addUser
addUser
环绕结束：执行目标方法之后，模拟关闭事务...
最终通知：模拟方法结束后的释放资源...
后置通知：模拟记录日志...,被织入增强处理的目标方法为：addUser
```

&#160;

----------

# Appendix

## Sample Code

[Java](https://github.com/937447974/Java)

## Related Documentation

[spring-framework-reference](https://docs.spring.io/spring/docs/5.0.0.RELEASE/spring-framework-reference/core.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-10-19 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974