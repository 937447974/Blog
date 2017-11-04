# 1 简介

MyBatis 是一个支持普通 SQL 查询、存储过程以及高级映射的持久化框架，它消除了几乎所有的 JDBC 代码和参数的手动设置以及对结果集的检索，并使用简单的 XML 或注解进行配置和原始映射，用以将接口和 JAVA 的 POJO 映射成数据库中的纪录，使得 Java 开发人员可以使用面向对象的编程思想来操作数据库。

MyBatis 的工作原理如下所示

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017110201.png)

核心对象主要涉及 SqlSessionFactory 和 SqlSession。

1. SqlSessionFactory: 单个数据库映射关系经过编译后的内存镜像，其主要作用是创建 SqlSession。
2. SqlSession: 应用程序与持久层支架执行交互操作的一个单线程对象，其主要租用是执行持久化操作。SqlSession 对象包含了数据库中所有执行 SQL 操作的方法，由于其底层封装了 JDBC 连接，所以可以直接使用其实例来执行已映射 SQL 语句。

> Exceutor 执行器，主要 SqlSession 的 SQL 执行器，我们制作的 plugins 插件就是在这里发挥作用。

# 2 整合 Spring

2017110401.png

## 2.1 XML 映射配置文件

MyBatis 提供了我们如下 xml 配置属性。

1. properties 属性
2. settings 设置
3. typeAliases 类型别名
4. typeHandlers 类型处理器
5. objectFactory 对象工厂
6. plugins 插件
7. environments 环境
	1. environment 环境变量
		1. transactionManager 事务管理器
		2. dataSource 数据源
8. databaseIdProvider 数据库厂商标识
9. mappers 映射器

项目使用 Spring 整合配置后，我们只需要使用极少数的配置，主要是 settings、typeAliases 和 plugins。其中相关配置的顺序是不可变的。

这里配置了 mybatis-config.xml 文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC
        "-//mybatis.org//DTD Config 3.0//EN"
        "http:/mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <setting name="logImpl" value="LOG4J"/>
    </settings>
    <typeAliases>
        <package name="com.mybatis.po"/>
    </typeAliases>
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor"/>
    </plugins>
</configuration>
```




# 3 Mapper 映射

&#160;

----------

# Appendix

## Sample Code

[Java](https://github.com/937447974/Java)

## Related Documentation

[mybatis](http://www.mybatis.org/mybatis-3/zh/index.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-10-26 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974