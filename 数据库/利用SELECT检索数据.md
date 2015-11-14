[MAC系统安装MySql](https://github.com/937447974/Blog/blob/master/数据库/MAC系统安装MySql.md)

[数据库的准则(范式)](https://github.com/937447974/Blog/blob/master/数据库/数据库的准则(范式).md)

[SQL基础](https://github.com/937447974/Blog/blob/master/数据库/SQL基础.md)

[利用SELECT检索数据](https://github.com/937447974/Blog/blob/master/数据库/利用SELECT检索数据.md)

[SQL内置函数](https://github.com/937447974/Blog/blob/master/数据库/SQL内置函数.md)

-----

>注意：本篇博文主要讲解oracle上的sql知识，然后是在mysql做测试。
>sql语法是不区分大小写的。

当在数据库的表中存入数据后，就可以查询这些已经存入的数据。在开发过程中，我们绝大部分的工作就是在做查询操作。查询数据需要用到select语句，可以使用不同复杂程度的查询语句来检索需要的数据。

#1 数据库

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

![数据库](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111215.png)

#2 查询数据必备SELECT

Select关键字表示数据的检索，它由一系列的子句组成，最终检索出来的数据由子句决定的。也就是说，检索出来的数据必须满足所有子句的限制。select语句按照复杂程度可以分为简单查询、where条件查询、多表查询、子查询等。

##2.1 SELECT语句语法

select语句是日常使用最多的语句，select语句的主要语法结构如下：

```sql
select [distinct|all] select_list
from table_name
[where_clause]
[group_by_clause]
[having condition]
[order_by_clause]
```

- select:查询关键字；
- [distinct|all]：描述列表字段中的数据是否取出重复记录；
- select_list：需要查询的字段列表；
- from：数据的来源；
- `[group_by_clause]`：查询的where条件语句；
- `[having condition]`：group by子句部分；
- `[order_by_clause]`：having子句部分；
- `[order_by_clause]`：排序

##2.2 获取指定字段的数据

获取表中指定字段的数据，就是指定表中的某几个字段，然后使用select语句查询。这里获取name字段。

```sql
-- 查询name字段
select name from user;
```

##2.3 获取所有字段的数据

要想查看表中所有字段的数据，最简单的方法就是利用“*”来查询。

```sql
select * from user;
```

你还可以写出所有字段名查询，效果一样。

```sql
select id,name,qq,age,createTime from user;
```

##2.4 使用别名替代表中的字段名

表中的字段名称都是英文的，这会给英文不好的客户查看数据带来不便。在select语句中允许我们指定别名，指定别名可以利用as关键字。

查询用户名和qq号码并加上别名。

```sql
select name as 用户名, qq as QQ from user;
```

##2.5 使用表达式操作查询的字段

可以针对某个列使用表达式，这样查询出来的结果就是修改后的数据，但是数据库中的数据不会被修改。

所有用户的岁数增加10岁。

```sql
select age+10 as newAge from user;
```

##2.6 使用函数操作查询的字段

查询过程中检索的列允许使用函数对其操作，如果仅仅是查询，那么更多的是利用函数对数据进行类型转换。

查询qq号码的前5位。

```sql
select qq,substr(qq,1,6) as 截取后的qq号码
from user;
```

##2.7 去除检索数据中的重复记录

当查询数据时有可能遇到重复的记录，这给统计有效数据造成了一定的影响，利用distinct关键字可以去除重复的数据。

去除重复的用户name和qq

```sql
select distinct name, qq
from user;
```

#3 检索出来的数据排序

要从海量的数据中找出我们模糊记忆中的数据，使用排序是个不错的选择，可以考虑让查询出来的记录根据某个字段进行排序操作。

##3.1 使用排序的语法

排序需要放置在select语句的后面，不管有什么条件限制，排序关键字只能在最后一项。下面是排序的语法，对应前面基本语法中的`order_by_clause`项。

```sql
order by
{expr | position | c_alias}
{asc | desc}
{nulls first | nulls last}
 [, {expr | position | c_alias}
    {asc | desc}
    {nulls first | nulls last}
 ]...
```

- order by：排序关键字。
- expr：表达式。
- position：表中列的位置。
- c_alias：别名。
- [asc | desc]：升序或降序。
- nulls first | nulls last：对空字段的处理方式。
- 可以根据多个字段排序。

##3.2 使用升序和降序来处理数据

可以使用升序方式或降序方式对查询出来的数据进行排序，如果对某个字段使用了order by子句而不指定排序方式，那么它将默认以升序的方式排序，即默认asc模式。

查询所有数据，以年龄降序排列。

```sql
select * from user
order by age desc;
```

##3.3 排序时对NULL值的处理

null值在排序过程中是个比较特殊的值类型，默认情况把它看成最大值。也就是，当排序的记录中出现null值时，默认情况下，升序排列它在最后，降序排列它在首位。mysql和oracle相反，null默认是最小值。

插入空数据。

```sql
insert into user(name, age) values('阳君', 26);
insert into user(name, qq) values('阳君', '937447974');
```

以qq字段做升序排列，null的放在首位。

```sql
select * from user
order by qq nulls first;
```

在mysql中写法如下：

```sql
select * from user
order by (1- isnull(qq)), qq;
```

##3.4 使用别名作为排序字段

为user表中的age指定别名，并使用该别名做降序排列。

```sql
select age as 年龄
from user
order by 年龄 desc;
```

##3.5 使用表达式作为排序字段

查询中允许使用表达式处理字段数据，其实也允许使用表达式作为排序字段。

user表中的age增加10，然后做排序。

```sql
select age, age+10 
from user
order by age+10;
```

##3.6 使用字段的位置作为排序字段

排序时允许使用观察下列表中字段的位置来作为排序手段，这么做有两个好处：查询方便和防止使用union时出现错误。

查询id、name和age，并利用age所在位置做排序。

```sql
select id,name,age
from user
order by 3 desc;
```

##3.7 使用多个字段排序

排序非常灵活，它不仅可以利用单一字段进行排序，还能利用多个字段进行排序，最后查询出来的数据是综合排序后的数据。多个字段排序的操作过程如下：

1. 按照第一个字段进行排序。
2. 在此基础上按照第二个字段排序。
3. 如果后面还有排序字段，那么这个过程会不断重复。

查询所有数据，并利用qq降序和age降序排序。

```sql
select * from user
order by qq desc, age desc;
```

#4 使用WHERE子句设置检索条件

select...from是一个基本的查询语句，它会无差别地返回所有的值，但我们希望检索出来的数据满足某个甚至多个条件，而利用where子句可以达到我们的目的。

where子句就像一个筛选器，它对from子句返回的结果进行筛选，每条记录都会按照条件进行判断，如果符合条件，则该记录作为查询结果的一部分，如果不符合条件则不会返回。

where条件子句中可以使用的操作符，主要有关系操作符、比较操作符和逻辑操作符。

1. 关系操作符包括：<、<=、>、>=、=、!=、<>.
2. 比较操作符包括：
 - is null：如果操作符为null返回true。
 - like 模糊比较字符串值。
 - between...and...：验证值是否在范围之内。
 - in：验证操作数在设定的一系列值中。
3. 逻辑操作符：
 - and：两个条件都必须满足。
 - or：只要满足两个条件中的其中一个。
 - not：与某个逻辑值取反。
 
简单的where条件语句一般只有一个限制条件，如果一个条件不行可以使用多个条件用逻辑操作符相连接。

##4.1 查询中使用单一条件

###4.1.1 查询中使用“>”

查询age>25的用户

```sql
select * from user
where age > 25;
```
 
###4.1.2 查询中使用“<>”

查询age不为25的用户

```sql
select * from user
where age <> 25;
```

利用“<>”作为查询条件时，可以使用“!=”替换；字符串类型的数据需要使用单引号括起。

###4.1.3 查询中使用函数

查询qq为“93744”开头的用户

```sql
select * from user
where substr(qq,1,5) = '93744';
```

##4.2 查询中使用多个条件限制

查询条件中除了单一的条件也可以设置多个条件，但是这些条件需要使用逻辑操作符连接起来。例如，and表示多个条件需要同时满足，其中一个条件不满足，那么该记录就不会返回到查询结果中。

查询qq为“93744”开头以及年龄在22到25岁之间的用户。

```sql
select * from user
where substr(qq,1,5) = '93744' and (age between 22 and 25);
```

##4.3 模糊查询数据

当并不能确切地了解查询条件，而是只了解查询条件的一部分时，或者想检索出包含特定字符的数据时，可以利用模糊查询。

使用模糊查询的关键字是like，它和两个通配符一起使用，才能实现模糊查询的功能。用这两个通配符可以代替模糊的部分，它们分别是：

- _：可以代替一个字符。
- %：可以代替多个字符。

检索qq为“93744”开头的用户。

```
select * from user
where qq like '93744%';
```

##4.4 查询条件限制在某个列表范围之内

某种情况下，要求查询条件从给定的值中选取，这时就可以利用in关键字来实现这个功能。它的语法是in(list),其中list是值列表。

查询age为22和23的用户。

```sql
select * from user
where age in ('22','23');
```

利用in检索出来的数据同分别利用“22”和“23”查询出来的数据是一样的。如果in前面使用not关键字，那么检索出来的数据将为age不为22且不为23的用户。


##4.5 专门针对NULL值的查询

数据库中的数据不会是完美的，更多的时候，由于种种原因，会存在垃圾数据和null数据。如果要检索null数据，将如何操作呢？利用 “is null”就可以达到目的，而利用”is not null“就可以检索非null的数据。

查询qq为null的用户

```sql
select * from user
where qq is null;
```

#5 GROUP BY和HAVING子句

group by子句和having子句同where不一样，它们两个都用于组的查询。使用分组可以统计数据，例如利用group by配合分组函数，可以查询相同年龄的用户人数。

##5.1 GROUP BY子句语法及使用

group by用于归纳汇总相关数据，它不属于where子句。也就是说，group by子句可以直接在from后面，也可以在where条件的后面。

插入数据

```sql
insert user(name, age, createTime) values('阳君', 25, '2015-11-12');
insert user(name, age, createTime) values('阳君', 23, '2015-11-12');
insert user(name, age, createTime) values('阳君', 24, '2015-11-12');
insert user(name, age, createTime) values('阳君', 25, '2015-11-12');
insert user(name, qq, age, createTime) values('阳君', '937447974', 24,' 2015-11-12');
insert user(name, qq, age, createTime) values('阳君', '937447974', 25, '2015-11-12');
```

###5.1.1 单一字段分组

查询相同年龄的人的人数

```sql
select age, count(*) from user
group by age;
```

###5.1.2 根据多个字段分组

查询相同年龄及创建时间相同的用户人数

```sql
select age, createTime, count(*) from user
group by age, createTime;
```

###5.1.3 where和分组混用

查询相同年龄及创建时间相同且qq号码存在的用户人数

```sql
select age, createTime, count(*) from user
where qq is not null
group by age, createTime;
```

##5.2 HAVING子句的使用

having子句通常和group by子句一起使用，限制搜索条件。它和where子句不一样，having子句与组有关，而不与单个的值有关。在group by子句中，它会作用于group by创建的组。

查询不同创建时间的用户的平局年龄，并列出平均年龄高于23的用户。

```sql
select createTime, avg(age) from user
group by createTime
having avg(age) > 23;
```

从语句上可以看出having与where的区别，having对group by子句负责，而where对from负责。

#6 使用子查询

什么是子查询?子查询就是嵌套查询，它是嵌套在另外一个语句中的select语句。where后面的条件不是一个确切的值或表达式，而是另外一个查询语句的查询结果。子查询不仅仅出现在select语句中，也会出现在delete和update语句中，它本质上是where后的一个条件表达式。

在前面的user表中，不知道你发现没有，其实会出现冗余数据，因为一个人可能有一个或多个qq号码。

下面重新设计数据库，生成两张表，一张user表，一张qq表。

user表的结构如下。

| 字段名 | 中文释义 | 数据类型 |
| :---- | :---- | :---- |
| id | 用户的id、唯一、自增、主键 | int |
| name | 用户名 | varchar(20) |
| age | 年龄 | int |

qq表的结构如下。

| 字段名 | 中文释义 | 数据类型 |
| :---- | :---- | :---- |
| id | 表id、唯一、自增、主键 | int |
| user_id | 用户id,外键，指向user表的id字段 | int |
| qq | QQ号码 | varchar(15) |

```sql
-- 删除表
drop table if exists qq;
drop table if exists user;
-- 创建user表
create table user (
  id int auto_increment,
  name varchar(20),
  age int, 
  PRIMARY KEY (id),
  constraint unique(id)
);
-- 创建qq表
create table qq (
  id int auto_increment,
  user_id int not null,
  qq varchar(15),
  primary key(id),
  constraint foreign key(user_id) references user(id) on delete cascade
);
-- 插入数据
insert into user(name, age) values('阳君', 24);
insert into user(name, age) values('阳君1', 25);
insert into user(name, age) values('阳君2', 25);
insert into user(name, age) values('阳君3', 26);
insert into user(name, age) values('yangjun', 25);
insert into user(name, age) values('yangjun2', 26);
insert into user(name, age) values('yangjun3', 22);
insert into qq(user_id, qq) values('1', '937447974');
insert into qq(user_id, qq) values('1', '937447975');
insert into qq(user_id, qq) values('2', '937447976');
```

现在你发现冗余相对减少了，而且表之间的关系也很清楚。

##6.1 子查询

子查询允许返回单行数据，也允许返回多行数据。

如果返回的是单行数据（不管是普通查询还是分组查询），那么这是逻辑上最简单的子查询嵌套查询语句。它和在where条件中使用单一或多个条件限制的操作方法一致。

查询用户名为“阳君”的qq号码。

```sql
select * from qq
where user_id = (select id from user where name = '阳君');
```

如果子查询返回的值为多行值，那么需要用到IN关键字，此时IN的用户和前面介绍的方式一致。除此之外，也可以使用量化比较关键字some、any、all，这些需要配合<、<=、=、>、>=使用。它们所表示的含义如下：

- any：表示满足子查询结果的任何一个即可。
- some：可以认为和any含义相同。
- all： 表示满足子查询结果的所有条件。

###6.1.1 in示例

查询所有存在qq号码的用户。

```sql
select * from user
where id in (select user_id from qq);
```

###6.1.2 any示例

```sql
select * from user
where age < any (select age from user);
```

会显示部分数据

###6.1.3 some示例

some的用法和any一样，只不过any多用于在非'='的环境中。

```sql
select * from user
where age < some (select age from user);
```

###6.1.4 all示例

```sql
select * from user
where age < all (select age from user);
```

没有数据显示。

#7 连接查询

关系型数据库中允许表和表之间存在关系，这种关系可以把两个甚至多个表的数据联系在一起。

利用这种关系，可以查询符合条件的数据，这些数据将是一套符合实际业务逻辑的数据，而数据中这些表和表之间的关系将不存在。换句话说，获取真实世界的原始数据后，根据某种规则把它们拆分成了各种独立的数据，假如想从数据库中再次获取原始数据，那么需要依靠当初拆分时的规则。而这种规则也可以看成表和表之间的联系，要再次实现这种联系，需要用到连接查询。连接分为内连接、外连接和全连接，还有一种叫做自连接，其中最常用的是内连接和外连接。

##7.1 最简单的连接查询

最简单的连接查询时利用逗号完成的，它利用逗号把form后的表名隔开，这就构成了最简单的连接查询。

###7.1.1 最简单的连接查询

连接user表和qq表。

```sql
select * from user, qq;
```

利用这种方式查询数据将得到两个表的笛卡尔积，也就是说得到两个表中记录数的乘积。

##7.2 内连接

内连接也称为简单连接，它会把两个或多个表进行连接，只能查询出匹配的记录，不匹配的记录将无法查询出来。内连接中最常用的就是等值连接和不等值连接。

###7.2.1 等值连接

连接条件中使用“=”连接两个条件的表。

查询user表和qq表中，userID一致的数据。

```sql
select * from user u, qq q
where u.id = q.user_id;
-- 或
select * from user u 
inner join qq q on u.id = q.user_id;
```

>内连接中inner可以不写，sql会自动判断。

###7.2.2 不等值连接

不等值连接就是之连接条件中使用>、>=、<=、<、!=、<>、between...and...、in等连接两个条件的列表，但这种方式通常需要和其他等值运算一起使用，否则检索出来的数据很可能没有实际意义。

查询低于平均年龄的用户

```sql
select * from user u
join (select avg(age) as avg_age from user) uv
on u.age < uv.avg_age;
```

##7.3 自连接

所谓自连接，就是把自身表的一个引用作为另一个表来处理，这样就能获取一些特殊的处境。

获取小于平均年龄的用户。

```sql
select u.name,u.age from user u, (select avg(age) as avg_age from user) uc
where u.age < uc.avg_age;
-- 或
select u.name,u.age from user u 
join (select avg(age) as avg_age from user) uc
on u.age < uc.avg_age;
```

##7.4 外连接

外连接分为左外连接、右外连接、全外连接。它们所表示的含义如下：

- 左外连接：又称为左向外连接。使用左外连接的查询，返回的结果不仅仅是符合连接条件的行记录，还包含了左边表中的全部记录。
- 右外连接：又称为右向外连接。它与左外连接相反，将右边的表中所有数据与左表进行匹配，返回的结果除了匹配成功的记录，还包含了右表中未匹配成功的记录，并在其左表对应的列补空值。
- 全外连接：返回所有匹配成功的记录，并返回左表未匹配成功的记录，也返回右表未匹配成功的记录。

###7.4.1 左外连接

左外连接使用关键字 left join ... on ...

```sql
select * from user u
left join qq q
on u.id = q.user_id;
```

###7.4.2 右外连接

右外连接和左外连接是相反的，它一右边的表为主表。右外连接使用关键字right join ... on ...

```
select * from user u
right join qq q
on u.id = q.user_id;
```

###7.4.3 全外连接

全外连接就是左外连接和右外连接的综合，它除了返回内连接匹配的数据外，也将返回两个表中不匹配的数据。全外连接使用的关键字是full join ... on ...

```sql
select * from user u
full join qq q
on u.id = q.user_id;
```

在mysql中没有全外连接，但是有另一种方式实现。

```sql
select * from user left join qq on user.id = qq.user_id
union
select * from user right join qq on user.id = qq.user_id;
```

>union关键字的作用是连接两个结果集，并去除重复的数据。

##7.5 多表连接

多表连接常用的关键字是union和union all。

###7.5.1 union

union：连接两个结果集的同时去除重复数据。

```sql
select * from user
union 
select * from user;
```

###7.5.1 union all

union all：连接两个结果集的同时不去除重复数据。

```sql
select * from user
union all
select * from user;
```

>union all比union查询的速度快。

##7.6 (+)的使用

在oracle中使用外连接，有一种比较特殊的表示方法，利用“(+)”表示外连接。虽然这种方式可以是实现外连接，当是oracle还是不建议使用。"(+)"主要在非主表的一方，并且使用where语句不能存在join关键字。

###7.6.1 左外连接使用(+)

```sql
select * from user u, qq q
where u.id = q.user_id(+);
```

###7.6.2 右外连接使用(+)

```sql
select * from user u, qq q
where u.id(+) = q.user_id;
```

#8 小结

本篇主要介绍了select语句的相关内容，包括select的语法、如何制定字段别名、如何在查询中使用函数等。为了使数据更加有条理，可以对检索出来的数据进行排序，利用order by子句可以根据某个字段或多个字段对查询数据进行排序。如果想检索出来特定的数据，那么可以利用where子句设置检索条件，检索条件可以是一个也可以是多个，只需利用and和or连接起来即可。关于子查询，它可以作为另一个select语句的查询条件，为动态地查询数据提供了条件。

&#160;

----------

#其他

##参考资料

[ORACLE从入门到精通](https://github.com/937447974/Blog/blob/master/学习资料/ORACLE从入门到精通.pdf)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-12 | 完成到“6 使用子查询”章节前 |
| 2015-11-13 | 完成文章 |
| 2015-11-14 | 增加7.5章节关于多表连接的介绍；增加其他文章的链接。 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog