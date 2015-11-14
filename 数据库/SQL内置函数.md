[MAC系统安装MySql](https://github.com/937447974/Blog/blob/master/数据库/MAC系统安装MySql.md)

[数据库的准则(范式)](https://github.com/937447974/Blog/blob/master/数据库/数据库的准则(范式).md)

[SQL基础](https://github.com/937447974/Blog/blob/master/数据库/SQL基础.md)

[利用SELECT检索数据](https://github.com/937447974/Blog/blob/master/数据库/利用SELECT检索数据.md)

[SQL内置函数](https://github.com/937447974/Blog/blob/master/数据库/SQL内置函数.md)

-----

>注意：本篇博文主要讲解oracle上的sql知识，然后是在mysql做测试。
>sql语法是不区分大小写的。

数据库中有多种内置函数，本篇文章主要介绍其中的两种，它们分别是单行函数和集合函数。这两种类型的函数在实际工作中使用的频率比较高。

单行函数是指当查询表或视图使每行都能返回一个结果，可用于select、where、order by等子句中。而集合函数是作用在多行记录上返回一个结果，可用于带group by或having子句的查询中。单行函数数量比较多，这里介绍几种常用的，它们分别是数值型函数、字符型函数、日期型函数、转换函数等。

介绍函数之前想简单介绍一下dual表。该表是数据库中真实存在的一个表，任何用户都可以读取，多数情况下可以用在没有目标的select查询语句中。该表很重要，千万不要删除，一旦删除，数据库将无法启动。

#1 数值型函数

数值类型函数可以输入数字，并返回一个数值。大多数可以达到小数点后38位。一部分则支持30位或36位小数。

##1.1 绝对值、取余、判断数值正负函数

###1.1.1 ABS(n)函数

abs(n)函数用于返回绝对值。该函数输入一个参数，参数类型为数值型，如果参数是可以转化为数值型的字符串，也可以。

```sql
select abs(100), abs(-100), abs('-100') from dual;
```

###1.1.2 mod(n2,n1)函数

mod(n2,n1)函数表示返回n2除以n1的余数。参数为任意数值或可以隐式转成数值的类型。如果n1为0，那么该函数将返回n2。

```sql
select mod(5,2), mod(8/3,5), mod(10,'5'), mod(1,0) from dual;
```

>在mysql中，遇到字符串结果为0；遇到n1为0结果为null。

###1.1.3 sign(n)函数

sign(n)函数返回参数n的符号。正输返回1，0返回0，负数返回-1。

```sql
select sign('9'), sign(-9), sign(0),sign(2*'3') from dual;
```

##1.2 三角函数

cos(n)函数。用于返回参数的余弦，n为弧度表示的角度。

```sql
select cos(3.1415926), cos('3.1415926') from dual;
```

与此类似函数还有如下几个。

- acos(n)：返回n的反余弦值。
- cosh(n)：返回n的双曲余弦值。
- sin(n)：返回n的正弦值。
- sinh(n)：返回n的双曲正弦值。
- asin(n)：返回n的反正切值。
- tan(n)：返回n的正切值。
- tanh(n)：返回n的双曲正切值。
- atan(n)：返回n的反正切值。

##1.3 返回以指定数值为准整数的函数

###1.3.1 ceil(n)函数

ceil(n)函数，其返回结果大于等于输入参数的最小整数。

```sql
select ceil(10), ceil('10.5'), ceil(-10.2) from dual;
```

###1.3.2 floor(n)函数

floor(n)函数，其返回结果是小于或等于参数的最大整数。同ceil(n)函数相反。

```sql
select floor(10), floor('10.5'), floor(-10.2) from dual;
```

##1.4 指数、对数函数

###1.4.1 sqrt(n)函数。

该函数返回n的平方根。n<0时oralce返回nan，mysql返回null。

```sql
select sqrt(100), sqrt('53.0'), sqrt(-100) from dual; 
```

###1.4.2 power(n2,n1)函数

利用该函数可以得到n2的n1次冥的结果。

```sql
select power(5,2), power('5',2), power(-5,2) from dual;
```

与其相近的函数有：exp(n)函数。表示返回e的n次冥。

###1.4.3 log(n1,n2)函数

该函数可以返回以n1为底n2的对数，n1是除1和0以外的任意正数。n2为正数。

```sql
select log(10,100), log(10.5,'100') from dual;
```

与其相近的函数有：ln(n)函数，表示返回n的自然对数。n要求大于0。

##1.5 四色五入截取函数

###1.5.1 round(for number)函数

该函数的具体原型是round(n,integer)。它将数值n四色五入成第二个参数指定的形式的十进制数。参数integer为正整数时，表示n被四色五入为integer位小数。如果该参数为负数，则n被四色五入至小数点向左integer位。

```sql
select round(100.23456789, 4), round(100.23456789,4.565), round(100.23456789,-4.56) from dual;
```

###1.5.2 trunc(for number)函数

该函数的具体原型是trunc(n,integer)。它把数值n根据integer的值进行截取，截取时和integer的正负有关。参数integer要求是整数，如果不是整数，那么它将自动截取为整数部分。当integer为正整数时，表示n将截取到integer位小数；如果integer为负数，则截取到小数点左第integer位，被截取部分用0代替。

```sql
select trunc(100.23456,4), trunc(100.23456,2.56), trunc(155.234,-2) from dual;
```

> mysql中无此函数

#2 字符型函数

以下函数全部接受的是字符型类型的参数，其中大部分返回字符类型数据，小部分返回数字类型数据。

##2.1 ASCII码与字符转换函数

###2.1.1 chr(n[using nchar_cs])函数

根据相应的字符集，把给定的ASCII码转换为字符。using nchar_cs指明字符集。

```sql
select chr(65), chr(66), chr(67) from dual;
```

mysql使用char代替

```sql
select char(65), char(66), char(67) from dual;
```

###2.1.2 ASCII(char)函数

返回参数首字母的ASCII码值。与chr函数相反。参数char的类型可以是char、varchar2、nchar或nvchar2。该返回值总是以用户使用的字符集为基础的，如果用户的数据库字符集是7位的ASCII值，那就得到一ASCII码。

```
select ascii('阳'), ascii('君') from dual;
```

##2.2 截取字符串长度函数

length函数。该函数可以得到指定字符串的字符长度，返回类型是数字。

```sql
select length('阳君') from dual;
```

##2.3 字符串截取函数

substr函数。该函数提供截取字符串的给你，而且该函数有很多的扩展形式，其具体语句结构是`[substr](char, position[, substring_length])`。各项参数表示含义如下：

- substr：以字符为单位。
- substrb：以字节为单位。
- substrc：以unicode字符为单位。
- substr2：以ucs2代码为单位。
- substr4：以ucs4代码为单位。
- char：原始字符串。
- position：要截取字符串的开始位置。初始为1，如果该值为负数，则表示从char的右边算起。
- substring_length：截取的长度。

```sql
select substr('阳君937447974',5,2), substr('阳君937447974',-5,2) from dual;
```

##2.4 字符串连接函数

concat(char1, char2)函数。该函数连接两个参数并返回。char2将连接到char1的尾部。效果和连接符“||”相似。

```sql
select concat('阳','君'), '阳'||'君' from dual;
```

>Mysql没有’||‘连接符，只能使用concat连接两个字符串。

##2.5 字符串搜索函数

instr函数。该函数可以让我们在指定的字符串中搜索是否存在另一个字符串。其具体语句结构是`[instr](string, substring[position[,occurrence]])`。各项参数表示含义如下：

- instr：以字符为单位。
- instrb：以字节为单位。
- instrc：以unicode字符为单位。
- instr2：以ucs2代码为单位。
- instr4：以ucs4代码为单位。
- string：原始字符串。
- position：搜索字符串的开始位置。默认为1，表示字符串左边第一个位置；如果该值为负数，则表示从char的右边算起。
- occurrence：substring第几次出现，默认是1。

```sql
select instr('阳君阳君937447974','阳君'), instr('阳君阳君937447974','阳君',-1) from dual;
```

> mysql中无法使用`instr('阳君阳君937447974','阳君',-1)`搜索，只支持`
select `搜索。

##2.6 字母大小写转换函数

###2.6.1 upper(char)函数

该函数将指定的参数全部转换成大写字母。

```sql
select upper('Yang Jun') from dual;
```

###2.6.2 lower(char)函数

该函数将指定的参数全部转换成小写字母。

```sql
select lower('Yang Jun') from dual;
```

###2.6.3 initcap(char)函数

该函数将参数的所有单词首字母转换成大写字母。

```sql
select initcap('yang jun') from dual;
```

>mysql无此函数。

##2.7 替换字符串函数

replace函数。函数具体语法结构是`replace(char,search_string[,replacement_string])`,是一个替换字符串的函数。函数中有三个参数，具体代表的含义如下：

- char：表示搜索的目标字符串。
- search_string：在目标字符串中要搜索的字符串。
- replacement_string：该参数可选，用她可替代被搜索到的字符串，如果该参数不用，则表示从char参数中删除search_string字符串。

```sql
select replace('阳君：qq','qq','937447974'), replace('阳君：qq','qq') from dual;
```

>在mysql中必须设置replacement_string参数。

##2.8 字符串填充函数

###2.8.1 rpad函数

函数具体语法结构是`rpad(expr1,n[,expr2])`,该函数功能是在字符串expr1的右边用字符串expr2填充，直到整个字符串长度为n时为至。如果expr2不存在，则以空格填充。

```sql
select rpad('阳君',18,'937447974') from dual;
```

###2.8.2 lpad函数

函数具体语法结构是`lpad(expr1,n[,expr2])`,该函数功能是在字符串expr1的左边用字符串expr2填充，直到整个字符串长度为n时为至。如果expr2不存在，则以空格填充。

```sql
select lpad('阳君',20,'937447974') from dual;
```

##2.9 删除字符串首尾指定字符的函数

###2.9.1 trim函数

该函数将删除指定的前缀和尾随的字符，默认删除空格。其具体语法结构是trim([leading|trailing|both][trim_character from]trim_source),各参数介绍如下：

- leading：删除trim_source的前缀字符。
- trailing：删除trim_source的后缀字符。
- both：删除trim_source前缀和后缀字符。
- trim_character：删除的指定字符，默认删除空格。
- trim_source：被操作的字符串。

```sql
select trim(trailing '7974' from '阳君937447974'), trim(' 阳君937447974 ') from dual;
```

###2.9.2 rtrim(char[,set])函数

与rpad函数相反，该函数会提供将char右边出现在set中的字符删除掉。如果set没有，则默认删除空格。

```sql
select rtrim(' 阳君937447974 '), rtrim(' 阳君937447974 ','74') from dual;
```

>mysql中，rtrim只能删除右边空格。

###2.9.3 ltrim(char[,set])函数

与rpad函数相反，该函数会提供将char左边出现在set中的字符删除掉。如果set没有，则默认删除空格。

```sql
select ltrim(' 阳君937447974 ') from dual;
```

>mysql中，ltrim只能删除左边空格。

#3日期型函数

日期类型的函数操作日期、时间类型的相关数据，并返回日期或数字类型的数据。

##3.1 系统时间、时间函数

###3.1.1 sysdate函数

该函数没有参数，可以得到系统的当前日期，是很常用的函数。下面示例演示了将得到的系统时间进行格式化。

```sql
select to_char(sysdate, 'YYYY-MM-DD HH24:MI:SS') from dual;
```

mysql用now()获取当前系统时间，也可以使用sysdate()获得。获取日期使用curdate()函数，获取时间使用curtime()。也就是说curdate()+curtime()=sysdate()/now()。

```sql
select now(), sysdate(), curdate(),curtime() from dual;
```

###3.1.2 systimestamp函数

该函数没有参数，返回系统时间，该时间包含时区信息，精确到微秒。返回类型为带时区信息的timestamp类型。

```sql
select systimestamp from dual;
```

>mysql无此函数。

##3.2 为日期加上指定月份函数

###3.2.1 add_months(date,integer)函数

该函数返回在指定日期上加一个月份数后的日期。各参数具体含义如下：

- date：指定的日期。
- integer：要加的月份数，该值如果为负数，则表示减去的月份数。

该函数有地方需要注意，当指定的日期是月的最后一天时，最后函数返回的结果页将是新月的最后一天。而如果新的月份比指定日期月份的天数少，则函数将自动回调有效函数。

```sql
select add_months(to_date('2015-11-30','YYYY-MM-DD'),1) from dual;
```

在mysql中使用统一的date_add(date,interval integer month)计算，该方法还可以计算时间，分钟等。

```sql
-- 时间处理
select 
now() as 当前时间,
date_add(now(), interval 1 day) as 增加一天,
date_add(now(), interval -1 day) as 减去一天,
date_add(now(), interval 1 hour) as 增加一小时,
date_add(now(), interval 1 minute) as 增加一分钟,
date_add(now(), interval 1 second) as 增加一秒,
date_add(now(), interval 1 microsecond) as 增加一毫秒
from dual;
-- 日期处理
select 
date_add(now(), interval 1 week) as 增加一周,
date_add(now(), interval 1 month) as 增加一月,
date_add(now(), interval 1 year) as 增加一年
from dual;
```

##3.3 返回指定月份最后一天函数

last_day(date)函数。该函数返回参数指定日期对应月份的最后一天。

oracle

```sql
select last_day(sysdate) from dual;
```

mysql

```sql
select last_day(now()) from dual;
```

##3.4 返回指定日期后一周的日期函数

next_day(date,char)函数。该函数返回当前日期向后的一周char的对应日期，char表示的是星期几。

```
select sysdate, next_day(sysdate,'星期一') from dual;
```

>mysql无此函数

##3.5 提取指定日期特定部分的函数

extract(datetime)函数。该函数可以从指定的时间当中提取到指定的日期部分，例如从给定的日期得到年、月、日等。

```sql
select
sysdate 当前时间,
extract(year from sysdate) 年,
extract(quarter from sysdate) 季度,
extract(month from sysdate) 月,
extract(week from sysdate) 周,
extract(day from sysdate) 天,
extract(hour from sysdate) 日,
extract(minute from sysdate) 分,
extract(second from sysdate) 秒,
extract(microsecond from sysdate) 毫秒
from dual;
```

只需将上面的sysdate改为now(),即可在mysql中执行。

##3.6 MySql 相关函数

###3.6.1 UTC日期时间函数

获取当前UTC日期时间函数有utc_date()、utc_time()和utc_timestamp()。因为我国位于东八时区，所以本地时间 = UTC 时间 + 8 小时。UTC 时间在业务涉及多个国家和地区的时候，非常有用。

```sql
select utc_date(), utc_time(), utc_timestamp() from dual;
```

###3.6.2 获取星期和月份名称

在mysql中可以获取某个时间对应的星期和月份名称。

- dayname(datetime)：返回周几。
- monthname(datetime)：返回月份。

```sql
select dayname(now()), monthname(now()) from dual;
```

###3.6.3 周位置函数

在mysql中关于周的函数有：

- week(date[,day])：返回date对应当年的第几周。如果有day，就在date的基础上增加day再获取第几周。
- yearweek(date)： 返回extract(year from date) + week(date)。

```sql
select now(), week(now()), yearweek(now()) from dual;
```

###3.6.4 天位置函数

在mysql中关于天位置的函数有：

- dayofweek(date)：返回某天在一周的位置(0 = Monday, 1 = Tuesday, …, 6 = Sunday)。
- dayofmonth(date)：返回日期参数在一月中的位置。
- dayofyear(date)：返回日期参数在一年中的位置。

```sql
select now(), dayofweek(now()), dayofmonth(now()), dayofyear(now()) from dual;
```

##4 集合函数

集合函数经常配合group by和having子句使用，当然它们也可以单独使用。该类型的函数都会忽略列值为null的值。

##4.1 测试数据

这里用到的测试数据，是上一篇博客的user表。下面就是相关介绍，如果你没看《[利用SELECT检索数据](https://github.com/937447974/Blog/blob/master/数据库/利用SELECT检索数据.md)》的博文，你可以重新导入。

user表的结构如下。

| 字段名 | 中文释义 | 数据类型 |
| :---- | :---- | :---- |
| id | 用户的id、唯一、自增、主键 | int |
| name | 用户名 | varchar(20) |
| age | 年龄 | int |


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
-- 插入数据
insert into user(name, age) values('阳君', 24);
insert into user(name, age) values('阳君1', 25);
insert into user(name, age) values('阳君2', 25);
insert into user(name, age) values('阳君3', 26);
insert into user(name) values('yangjun');
insert into user(name) values('yangjun');
```

##4.2 求平均值函数

avg(expr)函数。该函数可求取指定列的平均值，表示某组的平均值，返回数值类型。

```sql
-- 查询用户平均年龄
select avg(age) from user;
```

##4.3 求记录数量函数

count(expr)函数。该函数可以用来记录的数量或某列的个数。函数必须指定列名，或全选使用‘*’号。

```sql
select count(*) from user;
select count(age) from user;
```

运行两行代码会发现结果不一样，这是因为count(age)会忽略age=null的数据。

##4.4 返回最大、最小值函数

- max(expr)：返回指定列的最大值。
- min(expr)：返回指定列的最小值。

返回最小年龄和最大的年龄。

```sql
select min(age), max(age) from user;
```

##4.5 求和函数

sum(expr)函数。该函数会计算指定列的和，如果不使用分组，则函数默认把整个表作为一组。

求用户年龄之和

```sql
select sum(age) from user;
```

&#160;

----------

#其他

##参考资料

[ORACLE从入门到精通](https://github.com/937447974/Blog/blob/master/学习资料/ORACLE从入门到精通.pdf)

[MySQL 获得当前日期时间(以及时间的转换)](http://blog.sina.com.cn/s/blog_6d39dc6f0100m7eo.html)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-13 | 完成到“1.4 指数、对数函数”章节 |
| 2015-11-14 | 博文完成 |
| 2015-11-14 | 增加博文[MAC系统安装MySql](https://github.com/937447974/Blog/blob/master/数据库/MAC系统安装MySql.md)、[数据库的准则(范式)](https://github.com/937447974/Blog/blob/master/数据库/数据库的准则(范式).md)、[SQL基础](https://github.com/937447974/Blog/blob/master/数据库/SQL基础.md)、[利用SELECT检索数据](https://github.com/937447974/Blog/blob/master/数据库/利用SELECT检索数据.md)、[SQL内置函数](https://github.com/937447974/Blog/blob/master/数据库/SQL内置函数.md)的相关链接 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog