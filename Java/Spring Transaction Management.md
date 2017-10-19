实际开发中，操作数据库会涉及事务管理问题，为此 Spring 提供了专门用于事务管理的 API。Spring 的事务管理简化了传统的事务管理流程，并且在一定程度上减少了开发者的工作量。事务代理的调用方式如下所示。

![](https://docs.spring.io/spring/docs/5.0.0.RELEASE/spring-framework-reference/images/tx.png)

# 1 事务管理的核心接口

spring-tx 包就是 Spring 提供用于事务管理的依赖包，在包中能看见事务管理的核心接口 PlatformTransactionManager、TransactionDefinition 和 TransactionStatus。

## 1.1 PlatformTransactionManager

PlatformTransactionManager 接口就是 Spring 提供的平台事务管理器，主要用于管理事务。

```java
package org.springframework.transaction;

import org.springframework.lang.Nullable;

public interface PlatformTransactionManager {

	 // 获取事务状态信息
	TransactionStatus getTransaction(@Nullable TransactionDefinition definition) throws TransactionException;
	
	 // 提交事务
	void commit(TransactionStatus status) throws TransactionException;
	
	 // 回滚事务
	void rollback(TransactionStatus status) throws TransactionException;

}
```

PlatformTransactionManager 只是代表事务管理的接口，它并不知道底层是如何管理事务的，它只需要事务管理提供上面的3个方法，具体如何管理则由它的实现类 org.springframework.jdbc.datasource.DataSourceTransactionManager 来完成。

## 1.2 TransactionDefinition

TransactionDefinition 接口是事务定义的对象，该对象中定义了事务规则，并提供了获取事务相关信息的方法。

```java
package org.springframework.transaction;

import java.sql.Connection;
import org.springframework.lang.Nullable;

public interface TransactionDefinition {

	// 事务的传播行为
	int PROPAGATION_REQUIRED = 0;	
	int PROPAGATION_SUPPORTS = 1;	
	int PROPAGATION_MANDATORY = 2;	
	int PROPAGATION_REQUIRES_NEW = 3;	
	int PROPAGATION_NOT_SUPPORTED = 4;	
	int PROPAGATION_NEVER = 5;	
	int PROPAGATION_NESTED = 6;
	// 事务的隔离级别
	int ISOLATION_DEFAULT = -1;	
	int ISOLATION_READ_UNCOMMITTED = Connection.TRANSACTION_READ_UNCOMMITTED;
	int ISOLATION_READ_COMMITTED = Connection.TRANSACTION_READ_COMMITTED;
	int ISOLATION_REPEATABLE_READ = Connection.TRANSACTION_REPEATABLE_READ;
	int ISOLATION_SERIALIZABLE = Connection.TRANSACTION_SERIALIZABLE;
	// 事务的超时时间	
	int TIMEOUT_DEFAULT = -1;

	// 获取事务的传播行为
	int getPropagationBehavior();

	// 获取事务的隔离级别
	int getIsolationLevel();

	// 获取事务的超时时间
	int getTimeout();

	// 获取事务是否只读
	boolean isReadOnly();

	// 获取事务对象名称
	@Nullable
	String getName();

}
```

传播行为的种类如下

| 属性名称 | 值 | 描述 |
| ---- | ---- | ---- |
| PROPAGATION_REQUIRED | REQUIRED	 | 当前方法必须运行在一个事务环境当中，如果当前方法已处于事务环境中，则可以直接使用该方法；否则开启一个新事务后执行该方法。 |
| PROPAGATION_SUPPORTS | SUPPORTS | 当前事务处于事务环境中，则使用当前事务，否则不使用事务 |
| PROPAGATION_MANDATORY | MANDATORY | 调用该方法的线程必须处于当前事务环境中，否则将抛异常  |
| PROPAGATION_REQUIRES_NEW | REQUIRES_NEW | 当前方法已在事务环境中，则先暂停当前事务，在启动新的事务后执行该方法；如果当前方法不在事务环境中，则会启动一个新的事务后执行方法 |
| PROPAGATION_NOT_SUPPORTED | NOT_SUPPORTED | 当前方法的线程处于事务环境中，则先暂停当前事务，然后执行该方法 |
| PROPAGATION_NEVER | NEVER | 当前方法的线程处于事务环境中，抛异常 |
| PROPAGATION_NESTED | NESTED | 不管当前方法的线程是否处于事务环境中，都启动一个新的事务，然后执行该方法 |

在事务管理过程中，传播行为可以控制是否需要创建事务以及如何创建事务等。如果没有指定事务的传播行为，则默认的传播行为是 REQUIRED。

隔离级别的种类如下

| 属性名称 | 值 | 描述 |
| ---- | ---- | ---- |
| ISOLATION_DEFAULT | DEFAULT | 底层数据存储的默认隔离级别 |
| ISOLATION_READ_UNCOMMITTED | READ_UNCOMMITTED | 读取未提交，允许事务读取其他事务在提交到数据库之前产生的未提交更改 |
| ISOLATION_READ_COMMITTED | READ_COMMITTED | 读取已提交，只能读取其他事务提交到数据库中的数据 |
| ISOLATION_REPEATABLE_READ | REPEATABLE_READ | 可重复读，事务彼此隔离，一旦在某一事务中读取了数据库的一个值集，在后续的读取中都会读取到同样的值（除非此事务拿到这些数据的读写锁，并自行更改了数据）。 |
| ISOLATION_SERIALIZABLE | SERIALIZABLE | 交错发生的事务被堆起来，同一个时间点只能有一个事务具备访问目标数据的权利 |

## 1.3 TransactionStatus

TransactionStatus 接口是事务的状态，它描述了某一时间点上事务的状态信息。

```java
package org.springframework.transaction;

import java.io.Flushable;

public interface TransactionStatus extends SavepointManager, Flushable {

	// 是否是新事务
	boolean isNewTransaction();
	
	// 是否存在保存点
	boolean hasSavepoint();
	
	// 设置事务回滚
	void setRollbackOnly();
	
	// 是否回滚
	boolean isRollbackOnly();
	
	// 刷新事务
	@Override
	void flush();
	
	// 获取事务是否完成
	boolean isCompleted();

}
```

# 2 Annotation 事务管理

Spring 中的事务管理分为两种方式，传统的编程式事务管理和声明式事务管理。声明式事务管理分为基于 xml 和注解两种，实际开发过程中，我们都是使用基于注解的方式完成事务管理。

## 2.1 xml 配置

xml 配置的核心就是注册事务注解驱动。这里结合 MyBatis 设置相关配置信息。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!-- 读取db.properties -->
    <context:property-placeholder location="classpath:db.properties"/>

    <!-- 配置数据源 -->
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="maxTotal" value="${jdbc.maxTotal}"/>
        <property name="maxIdle" value="${jdbc.maxIdle}"/>
        <property name="initialSize" value="${jdbc.initialSize}"/>
    </bean>

    <!--配置mybatis工厂-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

    <!--事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 注册事务管理器驱动-->
    <tx:annotation-driven transaction-manager="transactionManager"/>
</beans>
```

## 2.2 @Transactional

在需要使用事务的类或方法上添加 @Transactional。如果在类添加，则表示事务的设置对整个类都起作用；如果在方法上添加，则表示事务的设置值对该方法有效。

使用 @Transactional 时，可以通过其参数配置事务详情。

```java
package org.springframework.transaction.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.core.annotation.AliasFor;
import org.springframework.transaction.TransactionDefinition;

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Transactional {

	// 目标事务管理器
	@AliasFor("transactionManager")
	String value() default "";
	@AliasFor("value")
	String transactionManager() default "";
	
	// 传播行为
	Propagation propagation() default Propagation.REQUIRED;
	
	// 隔离级别
	Isolation isolation() default Isolation.DEFAULT;
	
	// 超时时间
	int timeout() default TransactionDefinition.TIMEOUT_DEFAULT;
	
	// 是否只读
	boolean readOnly() default false;
	
	// 遇到特定异常强制回滚事务
	Class<? extends Throwable>[] rollbackFor() default {};
	String[] rollbackForClassName() default {};
	
		
	// 遇到特定异常强制不回滚事务
	Class<? extends Throwable>[] noRollbackFor() default {};
	String[] noRollbackForClassName() default {};

}
```
&#160;

----------

# Appendix

## Sample Code

[Java](https://github.com/937447974/Java)

## Related Documentation

[Data access and transaction management](https://docs.spring.io/spring/docs/5.0.0.RELEASE/spring-framework-reference/data-access.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-10-19 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974