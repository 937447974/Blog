>注意：本篇博文主要讲解oracle上的sql知识，然后是在mysql做测试。
>sql语法是不区分大小写的。

#1 SQL-数据库沟通的语言标准

SQL是每一个数据库都通用的语言，使用SQL可以在数据库中创建表、检索数据、操作数据，并对权限进行控制。SQL的主要功能就是在各种数据库间建立联系，进行沟通。

sql语言可以分为4类：

1. 定义要在数据库存储那些信息的数据定义语言（DDL）；
2. 对数据库中的表进行操作的数据操作语言（DML）；
3. 对数据库中的表进行检索的数据查询语言（DQL）；
4. 对数据库中对象进行权限管理的数据控制语言（DCL）；

##1.1 数据定义语言（DDL）

数据定义语言（DDL）是定义数据库中数据要如何存储的，包括对数据库中对象的创建、修改、删除的操作，这些对象主要有数据库、数据表、视图、索引等。

##1.2 数据操作语言（DML）

数据操作语言（DML）是对数据库表进行操作的，这些操作主要包括对数据表中的数据进行增加、删除、修改的操作，并且在操作时一次可以把表中的数据按条件进行多条或全部的处理，为数据库的使用提供方便。

##1.3 数据查询语言（DQL）

数据查询语言（DQL）是对数据库表中的数据进行查询的，查询时即可以查询一个表也可以进行多个表的查询，并且按不同的条件来检索数据，给数据库的查询统计工作带来了更多的便利。

##1.4 数据控制语言（DCL）

数据控制语言（DCL）是对数据库中的对象权限进行权限设置和取消等操作，但是只有数据库的系统管理员才有权力去执行对数据库对象权限的操作。使用DCL可以为数据库中不同的用户设置不同的权限，这样也能够提高数据库的安全性。

#2 数据库中支持的数据类型

数据类型主要分为字符型、数字型、日期类型和其他数据类型。

##2.1 字符型

| 数据类型 | 取值范围(字节) | 说明 |
| :---- | :---- | :---- | 
| varchar2 | 0~4000 | 可变长度的字符串 |
| nvarchar2 | 0~1000 | 用来存储Unicode字符集的可变长字符型数据 |
| char | 0~2000 | 用于描述定长的字符型数据 |
| nchar | 0~1000 | 用来存储Unicode字符集的定变长字符型数据 |
| long | 0~2GB | 用来存储变长的常字符串 |

>long类型很少使用，最常使用的字符数据类型就是varchar2。

##2.2 数字型

| 数据类型 | 取值范围 | 说明 |
| :---- | :---- | :---- |
| number(p,s) | p最大精度是38位（十进制） | p代表的是精度，s代表的是保留的小数位数；用来存储定常的整数和小数 |
| float | 用来存储126位数据（二进制） | 存储的精度是按二进制计算的，精度范围为二进制的1~126，在转化为十进制时需要乘以0.30103 |

##2.3 日期类型
| 数据类型 | 说明 |
| :---- | :---- |
| date | 用来存储日期和时间，范围在公元钱4712年1月1日到公元9999年12月31日 |
| timestamp | 用来存储日期和时间，与date类型的区别就是显示日期和时间时更精确。date类型精确秒，而timestamp的数据类型可以精确到小数秒。此外timestamp存放日期和时间还能够显示当前是上午还是下午 |

##2.4 其他数据类型

| 数据类型 | 取值范围(字节) | 说明 |
| :---- | :---- | :---- | 
| blob | 最多可以存放4GB | 存储二进制数据 |
| clob | 最多可以存放4GB | 存储字符串数据 |
| bfile | 大小与操作系统有关 | 用来把非结构化的二进制数据存储在数据库以外的操作系统中 |

#3 数据定义语言（DDl）

DDL主要包括数据库对象的创建（create）、删除（drop）和修改（alert）的操作。

##3.1 数据库操作

- show databases：列出数据库；
- use database_name：使用database_name数据库；
- create database data_name：创建名为data_name的数据库；
- drop database data_name：删除一个名为data_name的数据库；
- show tables：查看有那些表；
- show create table table_name: 查看创建table_name表的sql语句；
- describe table_name：查看table_name表的表结构。

##3.2 使用create语句创建表

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
describe user;
```

![DDl-1](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111101.png)

##3.3 使用Alter语句修改表

如果要对已经创建好的表进行修改，那么就需要使用alter talbe 语句来修改。修改表的基本语法如下：

```sql
alter table table_name
add column_name | modify column_name|drop column column_name;
```

- add: 用于向表中添加列；
- modify：用于修改表中已经存在的列的信息。
- drop column_name:删除表中的列，在删除表中的列时要经常加上cascade constraints，是要把与该列有关的约束也一并删除掉。

###3.3.1 增加字段

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

###3.3.2 修改字段类型

将address的类型改为number类型。

```sql
-- 修改列address类型
alter table user   
change address address int;
-- 查看表结构
describe user;
```

![DDl-4](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111102.png)

###3.3.3 修改字段名

运用上面的sql语句还可以修改字段名。

```sql
-- 修改address字段名为addresses
alter table user   
change address addresses int;
-- 查看表结构
describe user;
```

![DDl-5](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111106.png)

###3.3.4 修改字段顺序

```sql
-- 字段addresses移动字段userID后
alter table user   
modify addresses int after userID;
-- 查看表结构
describe user;
```

![DDl-6](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111103.png)

###3.3.5 删除字段

```sql
-- 删除字段addresses
alter table user drop addresses;
alter table user drop address2;
-- 查看表结构
describe user;
```

![DDl-7](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111107.png)

##3.4 使用Drop语句删除表

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

![DDl-8](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111108.png)

#4 约束的使用

约束是保证数据库表中数据的完整性和一致性的手段，在Oracle主要有5种约束，即主键约束、外键约束、唯一约束、检查约束和非空约束。

##4.1 主键约束

主键约束在每个表中只有一个，但是一个主键约束可以由数据表中多个列组成。主键约束有两种方式创建，一种是在创建表时设置约束，一种是创建后使用约束。下面还是使用前面的user表来讲解。

###4.1.1 创建表时创建主键约束

在创建表时就创建主键约束，只需要使用primary key（字段名）即可完成约束，这里我们让userID位主键。如果要使用多个字段设置为组合主键，只需primary key（字段名1，字段名2...）即可。

```sql
-- 创建user表并设置userID为主键
create table user (
    userID int,
    qq varchar(15),
    name varchar(20),
    createTime timestamp,
    primary key(userID)
);
-- 查看表结构
describe user;
```

![DDl-9](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111109.png)

###4.1.2 创建表后设置主键约束

在创建表时如果没有创建主键约束，可以再修改表时为表添加主键约束。添加主键约束的语法如下。

```sql
alter table table_name
add constraints constraint_name primary key(column_name);
```

- constraint_name:约束的名称。
- column_name：主键约束指定数据表中列名。

下面我们将qq设为主键。

```sql
-- 设置user表中的qq字段为主键
alter table user
add primary key(qq);
-- 查看表结构
describe user;
```

![DDl-10](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111111.png)

###4.1.3 移除主键约束

如果需要移除表中现有的主键约束，可以使用如下所示的语句完成：

```sql
alter table table_name
drop constraint constraint_name;
```

- constraint_name：要移除的约束名称，这个名称可以是在表中任意约束的名称。

下面我们移除user表中主键。

```sql
-- 移除user表的主键
alter table user
drop primary key;
-- 查看表结构
describe user;
```

![DDl-11](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111110.png)

##4.2 外键约束

外键约束可以保证使用外键约束的数据库列与所引用的主键约束的数据列一致，外键约束在一个数据表中可以有多个。也就是说，A表中字段id。B表中字段id的外键指向a表字段id，那么当插入b表数据时，id必须在a表中存在。

###4.2.1 创建表时创建外键约束

外键约束是建立在两张表中的约束，需要在创建表的语句后面加上如下语句：

```sql
constraint constraint_name foreign key (column_name)
reference table_name (column_name)
on delete cascade;
```

- constraint_name：创建的外键约束名字。
- foreign key (column_name)：指定外键约束的列名。
- reference：要引用的表名(列名)。
- on delete cascade：设置级联删除，当主键的字段被删除时，外键所对应的字段也被同时删除。

下面设计一张表，用于记录用户的上下班时间。这里我们创建work表，字段结构如下。

| 字段名 | 中文释义 | 数据类型 |
| :---- | :---- | :---- |
| id | 表id、唯一、自增、主键 | int |
| user_id | 用户id,外键，指向user表的id字段 | int |
| go_work_time | 上班时间 | timestamp |
| after_work_time | 下班时间 | timestamp |

```
-- 创建work表
create table work (
  id int auto_increment,
  user_id int not null,
  go_work_time timestamp,
  after_work_time timestamp,
  primary key(id),
  constraint foreign key(user_id) references user(id) on delete cascade
);
```

###4.2.2 在修改数据库表时添加外键约束

在已经存在的数据库表中也是可以添加外键约束的。添加外键约束是在alter table语句后面加上如下语句：

```sql
add constraint constraint_name foreign key (column_name)
reference table_name (column_name)
on delete cascade;
```

下面就使用上面的语句，完成对work表添加外键约束的操作。

```
alter table work
add constraint foreign key(user_id) 
references user(id) 
on delete cascade;
```

###4.2.3 移除外键约束

移除外键约束与移除主键约束的语法一致，这里以删除work表中的外键约束为例删除外键约束。

由于我们在上面没有设置外键约束名，你可以使用下面的语法查看外键约束名。

```sql
show create table work;
```

获得外键约束名“work_ibfk_1”后删除外键约束。

```sql
alter table work
drop constraint work_ibfk_1;
```

mysql与oracle删除外键约束的方式不一样，mysql中的写法如下。

```sql
alter table work
drop foreign key work_ibfk_1;
```

##4.3 CHECK约束

CHECK约束是检查约束，能够规定每一个列能够输入的值，以保证数据的正确性。

###4.3.1 创建表时添加check约束

创建check约束的语句就是在创建表的语句后面加上如下语句完成的。

```sql
constraint constraint_name check(condition)
```

- condition 检查约束的条件，检查约束的条件要建立在具体的字段中。

下面在创建user表时，设置userID必须大于0。

```sql
-- 创建user表并设置userID约束大于0
create table user (
    userID int,
    qq varchar(15),
    name varchar(20),
    createTime timestamp,
    check(userID > 0)
);
-- 查看表结构
describe user;
```

> 在mysql中，check约束不起任何作用。

###4.3.2 在修改数据表时添加check约束

在修改数据表时添加check约束的方法比较简单。

```sql
alter table table_name
add constraint constraint_name check(condition);
```

###4.3.3 移除check约束

移除check约束也与移除其他约束一样，只要知道check约束的名字，就快要移除check约束。

```sql
alter table table_name
drop constraint constraint_name;
```

##4.4 UNIQUE约束

unique约束称为唯一约束，可以设置在表中输入的字段值都是唯一的，这个约束和之前学习的主键约束非常相似。不同的是唯一约束在一个表中可以有多个，而主键约束在一个表中只能有一个。

###4.4.1 在创建表时添加unique约束

在创建表时可以为表中的字段直接添加unique约束，具体的创建方法是在创建表的语句后面加上下面的语句。

```sql
constraint constraint_name unique(column_name);
```

下面设置user表中的userID为唯一约束。

```sql
-- 创建user表并设置userID为唯一约束
create table user (
    userID int,
    name varchar(20),
    qq varchar(15),   
    createTime timestamp,
    constraint uk_userID unique(userID)
);
-- 查看表结构
describe user;
```

![DDl-12](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111201.png)

###4.4.2 在修改表时添加unique约束

修改表时添加unique约束也是在alter table语句后面加上如下语句完成的。

```sql
add constraint constraint_name unique(column_name);
```

下面添加user表的qq为唯一约束。

```sql
-- 修改表的qq为唯一约束
alter table user
add constraint uk_qq unique(qq);
-- 查看表结构
describe user;
```

![DDl](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111202.png)

也可使用

```sql
alter table user
add uk_qq unique(qq);
```

>如果不指定约束名，mysql默认用字段名为唯一约束名。


###4.4.3 移除唯一约束

移除union约束的方法也和移除其他约束一样。

```sql
alter table table_name
drop constraint constraint_name;
```

这里移除user表的qq唯一约束。

```sql
-- 移除user表中qq的唯一约束
alter table user
drop index uk_qq;
-- 查看表结构
describe user;
```

![DDl](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111203.png)

##4.5 NOT NULL 约束

Not null 约束就是非空约束，经常会在创建表时添加非空约束以确保字段必须要输入值。该约束和之前的约束不同，是直接在创建列时设置字段的非空约束。

###4.5.1 创建not null约束

创建not null约束的语法在创建表时就已经解释过了，这里创建user表时，设置name非空。

```sql
-- 创建user表并设置name字段非空
create table user (
    userID int,
    name varchar(20) not null,
    qq varchar(15)
);
-- 查看表结构
describe user;
```

![DDl](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111204.png)

###4.5.2 修改表时设置not null约束

在修改表时设置not null 约束，也不需要再使用add关键字来添加约束，只要使用modify关键字就可以设置表中字段的not null约束。

```sql
alter table table_name
modify column not null;
```

这里设置user表中qq非空。

```sql
-- 设置user表的qq字段非空
alter table user
modify qq varchar(15) not null;
-- 查看表结构
describe user;
```

![DDl](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111205.png)

###4.5.3 取消not null约束

对于非空约束不需要要删除，如果要取消某个列非空的约束，直接使用modify语句把该列的not null写成null即可。

```sql
alter table table_name
modify column null;
```

下面取消user表中qq字段非空的非空约束。

```sql
-- 取消user表的qq字段非空约束
alter table user
modify qq varchar(15) null;
-- 查看表结构
describe user;
```

![DDl](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111206.png)

#5 数据操作语言（DML）和数据查询语言（DQL）

DML也就是用来操作数据库中数据所使用的语言，对数据库中的数据操作无非就是对数据进行增加、删除、修改和查询的操作。

##5.1 INSERT添加数据

在创建好数据表之后，添加数据是首先要做的工作。在给表中添加数据时要与表中字段类型相匹配，也就是说字符串类型的字段只能添加字符串类型的数据。向表中添加数据的一般语法如下：

```sql
insert into talbe_name(column_name1,column_name2...)
values(data1,data2...);
```

- column_name1:字段名。
- data1：要添加的数据，与column_name1对应。

###5.1.1 直接添加数据

直接添加数据，使用上面的语法。这里我们为user表中添加一条记录。

```
-- 添加一条记录
insert into user(name,qq)
values('阳君', '937447974');
-- 查询所有数据
select * from user;
```

![DML](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111207.png)

###5.1.2 通过其他数据表向表中添加数据

如果在数据库中需要新创建一个数据表，但是这个表中的数据又与其他表中的数据相似，那么就可以直接把其他表中的数据添加到新创建的数据表中。具体语法如下：

```sql
insert into talbe_name1(column_name1, column_name2...)
select column_name1,column_name2... from table_name2;
```

- talbe_name1:要插入数据的表名。
- talbe_name2:数据的来源表。

>要确保列的个数和列的数据类型都一致。

这里将user表中的数据再次插入user表中。

```sql
-- 从表中添加数据
insert into user(name,qq)
select name,qq from user;
-- 查询所有数据
select * from user;
```

![DML](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111208.png)

上面介绍的这种添加数据的方式是目标数据表已经存在，也就是user表是创建好的，如果想不创建表就直接通过源数据表在添加数据的同时创建表也是可以的。具体语法如下：

```sql
create table table_name as 
select column_name1,column_name2... from source_table;
```

- table_name:要创建的表名；
- source_name:创建目标表时数据的来源表。这里可以指定字段，也可以用“*”代表来源表的所有字段。

这里我们通过这种方式创建user2表，字段取user表中的name和qq。

```sql
-- 创建表的同时插入数据
create table user2
as select name,qq from user;
-- 查询所有数据
select * from user2;
```

![DML](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111209.png)

##5.2 UPDATE修改数据

修改数据也是经常要使用的，在已经存在数据的表中修改数据使用update语句即可完成。

```sql
update table_name 
set column_name1=data1,column_name2=data2...
[where condition];
```

- column_name1:要修改数据列的字段名，可以是一个或多个；
- data1：要赋给字段的新值，这个值的数据类型要与表中字段的数据类型一致；
- where：条件，这里如果省略where语句，那么就意味着要修改表中该字段的所有值，如果加上where语句，则修改部分数据。

下面引入user表的初始数据。

```sql
-- 创建user表
create table user (
    userID int,
    name varchar(20),
    qq varchar(15)
);
-- 插入数据
insert into user(userID,name,qq) values('1','阳君','937447974');
insert into user(userID,name,qq) values('2','阳君','937447974');
-- 显示所有数据
select * from user;
```

![DML](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111211.png)

###5.2.1 修改表中指定字段的全部值

修改表中的全部值就是使用不带where字句的语句完成。下面修改user表中name为'yangjun';

```sql
-- user表中name字段的值都改为'yangjun'
update user
set name = 'yangjun';
-- 显示所有数据
select * from user;
```

![DML](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111210.png)

###5.2.2根据条件修改表中指定字段的值

根据条件修改表中的数据使用where字句来完成。下面修改userID=1的name值为'阳君'。

```sql
-- 修改userID=1的name值为'阳君'
update user
set name = '阳君'
where userID = 1;
-- 显示所有数据
select * from user;
```

![DML](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111212.png)

##5.3 DELETE删除数据

经常要删除数据表中一些没有用的数据，删除数据要使用delete关键字来完成。使用它可以根据条件删除指定的数据，也可以删除表中的全部数据。一般语法如下：

```sql
delete from table_name 
[where condition];
```

其中，[where condition]子句是可以省略的，如果省略了[where condition]子句，就意味着删除数据表中的全部数据，如果加上[where condition]子句就可以根据条件删除表中的数据。

###5.3.1 根据条件删除表中的记录

根据条件删除表中的记录就是使用[where condition]子句来完成，下面就删除user表中userID为1的记录。

```sql
-- 使用条件删除数据
delete from user
where userid = 1;
-- 显示所有数据
select * from user;
```

![DML](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111213.png)

###5.3.2 删除表中全部记录

删除表中的全部记录就是不使用[where condition]子句来完成操作。下面删除user表中的所有记录。

```sql
-- 删除所有数据
delete from user;
-- 显示所有数据
select * from user;
```

![DML](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111214.png)

##5.4 SELECT 查询数据

数据查询语言也称为DQL，这部分内会会在下一篇博文详细介绍。这里主要介绍SELECT语句的基本用法。SELECT的一般语法如下：

```sql
select column_name1,column_name2... 
from table_name
where[condition];
```

- column_name1：代表的是数据表中的字段名，可以是多个字段也可以是一个字段。还可以用“*”代替查询所有的字段。
- where[condition]：代表查询的条件。

###5.4.1 查询表中全部数据

这里查询user表中的全部数据。

```sql
-- 查询所有数据
select * from user;
```

###5.4.2 查询表中某一字段的数据

查询表中某一个字段的数据可以直接在select语句后面指定要查询的字段名。这里查询user表中的name字段。

```sql
-- 查询name字段
select name from user;
```

###5.4.3 根据条件查询数据

根据条件查询数据就是使用where子句来完成操作，下面查询user表中的userid=1的name字段。

```sql
-- 根据userid=1条件，查询user表中的name字段
select name
from user
where userid = 1;
```

##5.5 其他数据操作语句

在oracle中除了上面讲述的insert、update、delete、select语句之外，还有merge、truncate、lock table等语句。这里讲解常用的merge和truncate语句的使用。

###5.5.1 truncate语句

truncate语句和delete语句一样都是用来删除数据表中的数据的，二者的区别在于。

1. truncate只能删除全部数据，而delete可以删除部分数据；
2. truncate删除数据要比delete快。

具体的语法如下。

```sql
truncate table table_name;
```

###5.5.2 merge语句

merge语句与update语句的功能类似，都是修改数据表中的数据，但是使用merge可以同时进行增加和修改的操作。具体语法如下：

```sql
merge [into] table_name1
using table_name2
on (condtiion)
when matched then merge_update_clause
when not matched then merge_insert_clause;
```

- table_name1:要修改或添加的表。
- table_name2:参照的更新的表。
- condition：table_name1和table_name2之间的关系；
- merge_update_clause：如果参照表table_name2中的调节匹配，就执行更新操作的sql语句；
- merge_insert_clause：如果条件不匹配，就执行增加操作的sql语句。

> 这里merge_update_clause和merge_insert_clause都是可以省略的，当是在实际操作中会有一个，否则merge就没有意义了。

#6 数据控制语言（DCL）

数据控制离不开数据库的使用者，数据控制语言主要就是对数据库使用者赋予和撤销访问数据库的权限的设置，主要包括授予权限要使用的语句grant和收回权限的语句revoke。

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
| 2015-11-13 | 目录添加序号,添加章节“4.2 外键约束” |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog