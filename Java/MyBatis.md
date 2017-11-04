# 1 简介

MyBatis 是一个支持普通 SQL 查询、存储过程以及高级映射的持久化框架，它消除了几乎所有的 JDBC 代码和参数的手动设置以及对结果集的检索，并使用简单的 XML 或注解进行配置和原始映射，用以将接口和 JAVA 的 POJO 映射成数据库中的纪录，使得 Java 开发人员可以使用面向对象的编程思想来操作数据库。

MyBatis 的工作原理如下所示

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017110201.png)

核心对象主要涉及 SqlSessionFactory 和 SqlSession。

1. SqlSessionFactory: 单个数据库映射关系经过编译后的内存镜像，其主要作用是创建 SqlSession。
2. SqlSession: 应用程序与持久层之间执行交互操作的一个单线程对象，其主要作用是执行持久化操作。SqlSession 对象包含了数据库中所有执行 SQL 操作的方法，由于其底层封装了 JDBC 连接，所以可以直接使用其实例来执行已映射的 SQL 语句。

> Exceutor 执行器，主要 SqlSession 的 SQL 执行器，我们制作的 plugins 插件就是在这里发挥作用。

# 2 整合 Spring

## 2.1 Maven 导入

实际开发中使用 Maven 导入相关 jar 包，导入后如下图所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017110401.png)

> 其中 pagehelper 是一个分页插件

## 2.2 MyBatis XML 映射配置文件

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

项目使用 Spring 整合配置后，我们只需要使用极少数的配置，主要是 settings、typeAliases 和 plugins。其中相关配置的顺序是一定的，否则会报错。

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

> typeAliases 会给我们的 po 对象取别名。在 mapper 中可直接使用别名。

这里使用了 log4j 完成日志记录，mybatis 会自动加载 log4j.properties 文件，只需在项目根目录配置这个文件即可。

```
log4j.rootLogger=ERROR, stdout

# 指定有日志打印的包 com.mybatis.dao 模式:TRACE or DEBUG
log4j.logger.com.mybatis.dao=TRACE

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

## 2.3 Spring 整合 MyBatis XML 映射配置文件

Spring 加载 MaBatis 的 SqlSessionFactory 使用 SqlSessionFactoryBean，导入相关 mappers 映射文件使用 MapperScannerConfigurer。

spring-mybatis-config.xml 配置如下。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
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
    <tx:annotation-driven transaction-manager="transactionManager"/>

    <!--dao包-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.mybatis.dao"/>
    </bean>

</beans>
```

数据源配置在 db.properties，这有利于运维人员部署项目。

```xml
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?useSSL=false
jdbc.username=root
jdbc.password=
jdbc.maxTotal=30
jdbc.maxIdle=10
jdbc.initialSize=5
```

# 3 Mapper 映射

## 3.1 Mapper XML 文件

Mapper 主要配置 SQL 映射文件，可使用如下元素。

- cache：给定命名空间的缓存配置。
- cache-ref：其他命名空间缓存配置的引用，即将其他 mapper 的缓存引入当前 mapper。
- resultMap：是最复杂也是最强大的元素，用来描述如何从数据库结果集中来加载对象。
	-  association：一对一关联映射。
	-  collection：一对多关联映射。
- sql：可被其他语句引用的可重用语句块。
- insert：映射插入语句
- update：映射更新语句
- delete：映射删除语句
- select：映射查询语句

MyBatis 还支持在 Java 代码中直接写 SQL，但实际开发中建议使用 XML 配置，这有利于开发和维护。

## 3.2 动态 SQL

MyBatis 的强大特性之一便是它的动态 SQL，实际是基于 OGNL 的表达式。这样我们使用不到一半的语句即可完成 Mapper.xml 配置。

MyBatis 动态 SQL 中的主要元素如下

- `<if>`：判断语句，用于单条件分支判断。
- `<choose>(<when>,<otherwise>)`：相当于 java 中的 switch...case...default 语句。
- `<where>、<trim>、<set>`：辅助元素，用于处理一些 SQL 拼装、特殊字符问题。
- `<foreach>`：循环语句，相当于 java 中的 for...in。
- `<bind>`: 从 OGNL 表达式创建的一个变量，并将其绑定到上下文，常用于模糊查询的 SQL 中。

## 3.3 Mapper 实战

前面已经完成的 MyBatis 配置这里不在说明，下面主要介绍 Mapper 的实际应用。

### 3.3.1 数据库

数据库创建一个 mybatis 库。内部完成的关系是一个开发人员工作中主要只负责一种开发语言，但是他又同时掌握几种开发语言。

```sql
#建立库
DROP DATABASE mybatis;
CREATE DATABASE mybatis;
USE mybatis;

#建表
CREATE TABLE language (
  id   INT(32) PRIMARY KEY AUTO_INCREMENT,
  code VARCHAR(32) NOT NULL UNIQUE,
  name VARCHAR(50) NOT NULL
);
CREATE TABLE user (
  id            INT(32) PRIMARY KEY AUTO_INCREMENT,
  code          VARCHAR(32) NOT NULL UNIQUE,
  name          VARCHAR(50) NOT NULL,
  language_code VARCHAR(32),
  CONSTRAINT FOREIGN KEY (language_code) REFERENCES language (code)
);
CREATE TABLE user_language (
  id            INT(32) PRIMARY KEY AUTO_INCREMENT,
  user_code     VARCHAR(32) NOT NULL,
  language_code VARCHAR(32) NOT NULL,
  CONSTRAINT UNIQUE (user_code, language_code),
  CONSTRAINT FOREIGN KEY (user_code) REFERENCES user (code)
    ON DELETE CASCADE,
  CONSTRAINT FOREIGN KEY (language_code) REFERENCES language (code)
    ON DELETE CASCADE
);

# 查看表结构
DESCRIBE language;
DESCRIBE user;
DESCRIBE user_language;

# insert
INSERT language (code, name)
VALUES ('1', 'Java'), ('2', 'iOS'), ('3', 'JavasScript'), ('4', 'SQL');
INSERT user (code, name, language_code)
  VALUE ('937447974', '阳君', '1');
INSERT user_language (user_code, language_code)
VALUES ('937447974', '1'), ('937447974', '2');
```

相关 po 类这里就不在创建。

### 3.3.2 Mapper 接口

Mapper 接口使用 UserMapper。

```java
package com.mybatis.dao;

import com.mybatis.po.User;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

/**
 * UserMapper.java
 * <p>
 * Created by 阳君 on 2017/10/26.
 * Copyright © 2017年 mybatis. All rights reserved.
 */
public interface UserMapper {

    @Transactional(propagation = Propagation.MANDATORY)
    void insertUser(User user);
    @Transactional(propagation = Propagation.MANDATORY)
    void insertUsers(List<User> users);

    @Transactional(propagation = Propagation.MANDATORY)
    void deleteUser(String code);
    @Transactional(propagation = Propagation.MANDATORY)
    void deleteUsers(List<String> codes);

    @Transactional(propagation = Propagation.MANDATORY)
    void updateUser(User user);
    @Transactional(propagation = Propagation.MANDATORY)
    void updateUsers(List<User> users);

    List<User> selectUsers(User user);
    List<User> selectUserAndLanguage(User user);

}
```

### 3.3.3 Mapper xml

根据 Mapper 接口开发 xml 映射。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mybatis.dao.UserMapper">

    <!-- 缓存 -->
    <cache/>

    <!-- 增 -->
    <insert id="insertUser" parameterType="User">
        INSERT INTO user (code, name)
            VALUE (#{code}, #{name});
    </insert>
    <insert id="insertUsers">
        INSERT INTO user (code, name)
        VALUES
        <foreach collection="list" item="user" index="index" separator=",">
            (#{user.code}, #{user.name})
        </foreach>
    </insert>

    <!-- 删 -->
    <delete id="deleteUser" parameterType="String">
        DELETE FROM user
        WHERE code = #{code}
    </delete>
    <delete id="deleteUsers">
        DELETE FROM user
        WHERE code IN
        <foreach collection="list" item="code" open="(" separator="," close=")">
            #{code}
        </foreach>
    </delete>

    <!-- 改 -->
    <update id="updateUser" parameterType="User">
        UPDATE user
        <set>
            <if test="name != null and name != ''">
                name = #{name}
            </if>
        </set>
        WHERE code = #{code}
    </update>
    <update id="updateUsers">
        UPDATE user
        SET name =
        <foreach collection="list" item="user" open="case code" separator=" " close="end">
            when #{user.code} then #{user.name}
        </foreach>
        WHERE code IN
        <foreach collection="list" item="user" open="(" separator="," close=")">
            #{user.code}
        </foreach>
    </update>

    <!-- 查 -->
    <select id="selectUsers" parameterType="User" resultType="User">
        SELECT *
        FROM user u
        <where>
            <include refid="whereUserSql"/>
        </where>
        ORDER BY name
    </select>
    <select id="selectUserAndLanguage" parameterType="User" resultMap="userAndLanguageMap">
        SELECT
        u.id, u.code, u.name,
        l.code language_code, l.name language_name,
        ul.l_code languages_code, ul.l_name languages_name
        FROM user u, language l, (<include refid="userLanguagesSql"/>) ul
        <where>
            u.language_code = l.code
            AND u.code = ul.u_code
            <include refid="whereUserSql"/>
        </where>
        ORDER BY u.name
    </select>
    <!-- 结果映射 -->
    <resultMap id="userAndLanguageMap" type="User">
        <id property="id" column="id"/>
        <result property="code" column="code"/>
        <result property="name" column="name"/>
        <!-- 一对一 -->
        <association property="language" javaType="Language">
            <result property="code" column="language_code"/>
            <result property="name" column="language_name"/>
        </association>
        <!-- 一对多/多对多 -->
        <collection property="languages" ofType="Language">
            <result property="code" column="languages_code"/>
            <result property="name" column="languages_name"/>
        </collection>
    </resultMap>
    <!-- sql 片段-->
    <sql id="whereUserSql">
        <!-- 上下文绑定 -->
        <bind name="likeName" value="'%' + name + '%'"/>
        <if test="code != null and code != ''">
            AND u.code = #{code}
        </if>
        <if test="name != null and name != ''">
            AND u.name LIKE #{likeName}
        </if>
    </sql>
    <sql id="userLanguagesSql">
        SELECT
            u.code u_code,
            l.code l_code,
            l.name l_name
        FROM user u, language l, user_language ul
        WHERE ul.user_code = u.code
              AND ul.language_code = l.code
    </sql>

</mapper>
```

这里可以看出使用 xml 做sql映射，逻辑清晰便于维护。

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
| 2017-11-04 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974