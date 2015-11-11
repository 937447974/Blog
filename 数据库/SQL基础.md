>注意：本篇博文主要讲解oracle上的sql知识，然后是在mysql做测试。

#SQL-数据库沟通的语言标准

SQL是每一个数据库都通用的语言，使用SQL可以在数据库中创建表、检索数据、操作数据，并对权限进行控制。SQL的主要功能就是在各种数据库间建立联系，进行沟通。

sql语言可以分为4类：

1. 定义要在数据库存储那些信息的数据定义语言（DDL）；
2. 对数据库中的表进行操作的数据操作语言（DML）；
3. 对数据库中的表进行检索的数据查询语言（DQL）；
4. 对数据库中对象进行权限管理的数据控制语言（DCL）；

##数据定义语言（DDL）

数据定义语言（DDL）是定义数据库中数据要如何存储的，包括对数据库中对象的创建、修改、删除的操作，这些对象主要有数据库、数据表、视图、索引等。

##数据操作语言（DML）

数据操作语言（DML）是对数据库表进行操作的，这些操作主要包括对数据表中的数据进行增加、删除、修改的操作，并且在操作时一次可以把表中的数据按条件进行多条或全部的处理，为数据库的使用提供方便。

##数据查询语言（DQL）

数据查询语言（DQL）是对数据库表中的数据进行查询的，查询时即可以查询一个表也可以进行多个表的查询，并且按不同的条件来检索数据，给数据库的查询统计工作带来了更多的便利。

##数据控制语言（DCL）

数据控制语言（DCS）是对数据库中的对象权限进行权限设置和取消等操作，但是只有数据库的系统管理员才有权力去执行对数据库对象权限的操作。使用DCL可以为数据库中不同的用户设置不同的权限，这样也能够提高数据库的安全性。

#数据库中支持的数据类型

数据类型主要分为字符型、数字型、日期类型和其他数据类型。

##字符型

| 数据类型 | 取值范围(字节) | 说明 |
| :---- | :---- | :---- | 
| varchar2 | 0~4000 | 可变长度的字符串 |
| nvarchar2 | 0~1000 | 用来存储Unicode字符集的可变长字符型数据 |
| char | 0~2000 | 用于描述定长的字符型数据 |
| nchar | 0~1000 | 用来存储Unicode字符集的定变长字符型数据 |
| long | 0~2GB | 用来存储变长的常字符串 |

>long类型很少使用，最常使用的字符数据类型就是varchar2。

##数字型

| 数据类型 | 取值范围 | 说明 |
| :---- | :---- | :---- |
| number(p,s) | p最大精度是38位（十进制） | p代表的是精度，s代表的是保留的小数位数；用来存储定常的整数和小数 |
| float | 用来存储126位数据（二进制） | 存储的精度是按二进制计算的，精度范围为二进制的1~126，在转化为十进制时需要乘以0.30103 |

##日期类型
| 数据类型 | 说明 |
| :---- | :---- |
| date | 用来存储日期和时间，范围在公元钱4712年1月1日到公元9999年12月31日 |
| timestamp | 用来存储日期和时间，与date类型的区别就是显示日期和时间时更精确。date类型精确秒，而timestamp的数据类型可以精确到小数秒。此外timestamp存放日期和时间还能够显示当前是上午还是下午 |

##其他数据类型

| 数据类型 | 取值范围(字节) | 说明 |
| :---- | :---- | :---- | 
| blob | 最多可以存放4GB | 存储二进制数据 |
| clob | 最多可以存放4GB | 存储字符串数据 |
| bfile | 大小与操作系统有关 | 用来吧非结构化的二进制数据存储在数据库以外的操作系统中 |

#数据定义语言（DDl）

DDL主要包括数据库对象的创建（create）、删除（drop）和修改（alert）的操作。

##数据库操作

- show databases：列出数据库
- use database_name：使用database_name数据库
- create database data_name：创建名为data_name的数据库
- drop database data_name：删除一个名为data_name的数据库
- show tables：查看有那些表。
- DESCRIBE table_name：查看table_name表的表结构。

##使用create语句创建表

创建表使用create table语句完成。

```sql
create table table_name {
    column_name dataype [null|not null];
    column_name dataype [null|not null];
    [constraint]
}
```

- table_name:在数据库中创建的表的名称，在一个数据库中数据表名是不能重复的。
- column_name:表中的列名，列名在一个表中也是不能重复的。
- datatype：该列存放数据的数据类型。
- [null|not null]：允许该列为空或者不允许该列为空，在创建表时默认为允许该列为空。

下面利用上面的语句创建一个用户表user。商品信息表字段类型如下所示。

| 字段名 | 中文释义 | 数据类型 |
| :---- | :---- | :---- |
| userID | 用户的id | number(10) |
| name | 用户名 | varchar2(20) |
| qq | QQ号码 | varchar2(15) |
| createTime | 创建时间 | timestamp |

mysql 创建：

```sql
-- 创建user表
create table user (
    userID int(10),
    qq varchar(15),
    name varchar(20),
    createTime timestamp
);
-- 查看表结构
describe user；
```

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

##使用Alter语句修改表

如果要对已经创建好的表进行修改，那么就需要使用alter talbe 语句来修改。修改表的基本语法如下：

```sql
alter table table_name
add column_name | modify column_name|drop column column_name;
```

- add: 用于向表中添加列；
- modify：用于修改表中已经存在的列的信息。
- drop column_name:删除表中的列，在删除表中的列时要经常加上cascade constraints，是要把与该列有关的约束也一并删除掉。

###增加字段

在user表中增加列地址address，字段类型是varchar2。

```sql
-- 增加列address
alter table user
add address varchar(50);
-- 查看表结构
describe user;
```

![DDl-2](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111103.png)

你还可以指定添加到某个字段后

```sql
-- 增加字段address，并在userID字段后
alter table user
add address2 varchar(50) after userID;
-- 查看表结构
describe user;
```

![DDl-3](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111105.png)

###修改字段类型

将address的类型改为number类型。

```sql
-- 修改列address类型
alter table user   
change address address int;
-- 查看表结构
describe user;
```

![DDl-4](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111102.png)

###修改字段名

运用上面的sql语句还可以修改字段名。

```sql
-- 修改address字段名为addresses
alter table user   
change address addresses int;
-- 查看表结构
describe user;
```

![DDl-5](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111106.png)

###修改字段顺序

```sql
-- 字段addresses移动字段userID后
alter table user   
modify addresses int after userID;
-- 查看表结构
describe user;
```

![DDl-6](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111103.png)

###删除字段

```sql
-- 删除字段addresses
alter table user drop addresses;
alter table user drop address2;
-- 查看表结构
describe user;
```

![DDl-7](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111107.png)

##使用Drop语句删除表

在使用数据库中的表时经常需要删除一些不需要的表，删除表需要使用drop table 语句来完成。具体语句如下：

```sql
drop table (if exists) table_name;
```

删除表的语句非常简单，只需要指定要删除的表名，即可删除该表。if exists代表表存在时才删除，也可以不写。

下面就利用上面删除表的语句来删除user表。

```sql
-- 查看库中的存在的表
show tables;
-- 删除user表
drop table if exists user;
-- 查看删除user表后，库中的存在的表
show tables;
```


&#160;

----------

#其他

##参考资料

[ORACLE从入门到精通](https://github.com/937447974/Blog/blob/master/学习资料/ORACLE从入门到精通.pdf)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-10 | sql基础 |

&#160;

----------

版权所有：http://blog.csdn.net/y550918116j