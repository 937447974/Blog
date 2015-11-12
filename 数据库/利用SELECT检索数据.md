当在数据库的表中存入数据后，就可以查询这些意见存入的数据。在开发过程中，我们绝大部分的工作就是在做查询操作。查询数据需要用到select语句，可以使用不同复杂程度的查询语句来检索需要的数据。

#数据库

在讲解今天的知识前我们先创建测试数据。这里我们创建一个user表，字段结构如下。

| 字段名 | 中文释义 | 数据类型 |
| :---- | :---- | :---- |
| id | 用户的id、自增、主键 | int(20) |
| name | 用户名 | varchar(20) |
| qq | QQ号码 | varchar(15) |
| age | 年龄 | int |
| createTime | 创建时间 | timestamp |

使用mysql创建表：

```sql
-- 创建测试库
create database test;
-- 使用测试表
use test;
-- 创建user表
create table user (
    id int(20) auto_increment,
    name varchar(20),
    qq varchar(15),
    age int,
    createTime timestamp,
    primary key (id)
);
-- 查看表结构
describe user;
-- 增加数据
insert into user(name, qq, age, createTime) values('阳君', '937447974', 20, '2015-11-12');
insert into user(name, qq, age, createTime) values('阳君', '937447974', 21, '2015-11-13');
insert into user(name, qq, age, createTime) values('阳君', '937447974', 22, '2015-11-14');
insert into user(name, qq, age, createTime) values('阳君', '937447974', 23, '2015-11-15');
insert into user(name, qq, age, createTime) values('阳君', '937447974', 24, '2015-11-16');
insert into user(name, qq, age, createTime) values('阳君', '937447974', 25, '2015-11-17');
-- 查询所有数据
select * from user;
```



#查询数据必备SELECT

下面利用上面的语句创建一个用户表user。商品信息表字段类型如下所示。

数据库


&#160;

----------

#其他

##参考资料

[ORACLE从入门到精通](https://github.com/937447974/Blog/blob/master/学习资料/ORACLE从入门到精通.pdf)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-10 | sql基础 |
| 2015-11-12 | unique唯一约束、not null非空约束、数据操作语言（DML）和数据查询语言（DQL）、数据控制语言（DCL）章节 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog