> 数据库是用来**组织，存储和管理**数据的仓库

## 数据管理系统

> 用户可以对数据库中的数据进行**新增、查询、更新、删除**等操作

### 常见数据库及其分类

![image-20220724101859423](D:/Code/md/some-note-md/img/mysql/%E5%B8%B8%E8%A7%81%E6%95%B0%E6%8D%AE%E5%BA%93%E5%8F%8A%E5%85%B6%E5%88%86%E7%B1%BB.png)



#### 传统数据库类型的数据组织结构

分为**数据库、数据表、数据行、字段**这四大部分

![image-20220724110709640](D:/Code/md/some-note-md/img/mysql/%E4%BC%A0%E7%BB%9F%E5%9E%8B%E6%95%B0%E6%8D%AE%E7%BB%84%E7%BB%87%E7%BB%93%E6%9E%84.png)





## MYSQL

### 安装并配置

* MySQL Server 

  * 专门用来提供数据存储和服务的软件

* MySQL Workbench

  * 可视化的 MySQL 管理工具
  * 可以方便的操作存储在MySQL Server 中的数据




### 服务的停止和启动

* 停止服务
  * 需要管理员身份打开终端
  * 停止服务后无法登录

`net stop mysql80`

* 启动服务

`net start mysql80`



#### 在命令行中登录

```mysql
mysql -uroot -p149517
```

* `-u`表示用户，后面也可以添加空格
* `-p`表示密码，可以直接跟着写
* 也可输入-p后回车换行输入

##### 访问其他版本的mysql

```mysql
mysql -u root -P 13360 -p
```

* `-P`表示的是端口号
* 在其他版本使用不同的
* `-p`后面的空格表示的是密码，所以不能跟空格
* `-h`表示的是访问的地址
  * localhost 本机
* `-P`和`-h`如果在本机则可省略



#### 关系型数据库

> 建立在关系模型基础上，由多张相互连接的二维表组成的数据库

* 使用表存储数据，格式统一便于维护
* 使用SQL语句操作，标准统一，使用方便

#### 数据模型

![image-20220727223805101](D:/Code/md/some-note-md/img/mysql/%E6%95%B0%E6%8D%AE%E6%A8%A1%E5%9E%8B.png)



## SQL

#### 通用语法

1. SQL 语句可以多行或者多行书写，以分号结尾
2. SQL 语句可以使用 空格/ 缩进来增强语句的可读性
3. MYSQL数据库的SQL语句不区分大小写，关键字建议使用大写
4. 注释
   * 单行注释：--注释内容 或者 # 注释内容（mysql特有）
   * 多行注释：/* 注释内容 */



#### SQL分类

| 分类 | 全称                       | 说明                                                   |
| ---- | -------------------------- | ------------------------------------------------------ |
| DDL  | Data Definition Language   | 数据定义语言，用来定义数据库对象（数据库，表，字段）   |
| DML  | Data Manipulation Language | 数据操作语言，用来对数据库表中的数据进行**增删改**     |
| DQL  | Data Query Language        | 数据查询语言，用来查询数据库中**表的记录**             |
| DCL  | Data Control Language      | 数据控制语言，用来创建数据库用户，控制数据库的访问权限 |



### DDL

#### 数据库

##### 查询

* 查询所有数据库

```SQL 
SHOW DATABASES;
```

* 查询当前数据库

```SQL
SELECT DATABASE();
```

需要添加括号



##### 创建

```SQL
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];
```



##### 删除

```SQL 
DROP DATABASE [IF EXISTS] 数据库名;
```



##### 使用

```sql
USE 数据库名;
```



#### 表操作

##### 查询

* 查询当前数据库的所有表

```sql
SHOW TABLES;
```



* 查询表结构

```sql
DESC 表名;
```



* 查询指定的建表语句

```sql
SHOW CREATE TABLE 表名;
```



##### 创建

```sql
CREATE TABLE 表名(
	字段1 字段类型 [COMMENT 字段注释],
       字段2 字段类型 [COMMENT 字段注释],
       字段3 字段类型 [COMMENT 字段注释],
)[COMMENT 表注释];
```



**[...]为可选参数，最后一个字段后面没有逗号**

```sql
mysql> create table tb_user(
    -> id int comment 'id',
    -> name varchar(50) comment 'name',
    -> age int comment 'age',
    -> gender varchar(1) comment 'gender'
    -> ) comment 'user_name';
```

##### 修改

* 添加字段

```sql
ALTER TABLE 表名 ADD 字段名 类型(长度) [comment 注释] [约束];
```



* 修改数据类型

```sql
ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);
```



* 修改字段名和字段类型

```sql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束]
```



* 修改表名

```sql
ALTER TABLE 表名 RENAME TO 新表名;
```



##### 删除

* 删除字段

```sql
ALTER TABLE 表名 DROP 字段名;
```

* 删除表

```sql
DROP TABLE [IF EXISTS] 表名;
```

* 删除指定表，并重新创建该表

```sql
TRUNCATE TABLE 表名;
```



### Mysql 数据类型

#### 数值类型

| 类型         |                   大小                   | 范围（有符号）                                               | 范围（无符号）                                               | 用途            |
| :----------- | :--------------------------------------: | :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------- |
| TINYINT      |                 1 Bytes                  | (-128，127)                                                  | (0，255)                                                     | 小整数值        |
| SMALLINT     |                 2 Bytes                  | (-32 768，32 767)                                            | (0，65 535)                                                  | 大整数值        |
| MEDIUMINT    |                 3 Bytes                  | (-8 388 608，8 388 607)                                      | (0，16 777 215)                                              | 大整数值        |
| INT或INTEGER |                 4 Bytes                  | (-2 147 483 648，2 147 483 647)                              | (0，4 294 967 295)                                           | 大整数值        |
| BIGINT       |                 8 Bytes                  | (-9,223,372,036,854,775,808，9 223 372 036 854 775 807)      | (0，18 446 744 073 709 551 615)                              | 极大整数值      |
| FLOAT        |                 4 Bytes                  | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
| DOUBLE       |                 8 Bytes                  | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| DECIMAL      | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 | 依赖于M和D的值                                               | 依赖于M和D的值                                               | 小数值          |

#### 字符串类型

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

* char(10)
  * 即使存储了1位也会从存储为十个，
  * 其余将会补0
* varchar(10)
  * 根据输入字符的长度计算

#### 日期类型

| 类型      | 大小 ( bytes) | 范围                                                         | 格式                | 用途                     |
| :-------- | :------------ | :----------------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3             | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3             | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1             | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8             | '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'               | YYYY-MM-DD hh:mm:ss | 混合日期和时间值         |
| TIMESTAMP | 4             | '1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYY-MM-DD hh:mm:ss | 混合日期和时间值，时间戳 |

* 一张员工信息表
* ![image-20220728103524537](D:/Code/md/some-note-md/img/mysql/%E6%A1%88%E4%BE%8B%E8%A6%81%E6%B1%82.png)

```sql
create table emp(
    id int comment 'id',
    workno varchar(10) comment 'Employee job number',
    name varchar(10) comment 'Employee job name',
    gender char(1),
    age tinyint unsigned,
    idcard char(18),
    entrydate date
) comment 'staff table';
```



### DML

> 对数据表中的数据记录进行增删改操作

#### 添加数据(INSERT)

* 给指定字段添加数据

```SQL
INSERT INTO 表名(字段名1，字段名2,···) VALUES (值1，值2，···)；
```

* 给全部字段添加数据

```sql
INSERT INTO 表名 VALUES (值1，值2，···);
```

* 批量添加数据

```sql
INSERT INTO 表名(字段名1，字段名2,···) VALUES (值1，值2，···),值1，值2，···),值1，值2，···),；
```

```sql
INSERT INTO 表名 VALUES (值1，值2，···),值1，值2，···),值1，值2，···),；
```



**注意**

* 插入数据时候，指定的字段和值一一对应
* 字符串和日期型数据应包含在引号中
* 插入的数据大小，应该在字段的规定范围内



#### 修改数据(UPDATE)

* 修改数据

```sql
UPDATE 表名 SET 字段1 = 值1, 字段名2 = 值2，··· [WHERE 条件];
```

**修改语句的条件可以有也可以没有，如果没有条件，则会修改整个表的所有数据**



#### 删除数据(DELETE)

```sql
DELETE FROM 表名 [WHERE 条件];
```



**注意**

* delete 语句的条件可以有，也可以没有，如果没有条件，则会删除整个表的所有数据
* delete 语句不能删除某一个字段，（可以使用 update ）



### DQL

> 数据查询语言，用来查询数据库中的记录
>
> 关键字：`SELECT`

#### 基本查询

* 查询多个字段

```sql
SELECT 字段1，字段2，字段3，··· FROM 表名;
```

```sql
SELECT * FROM 表名;
```

尽量少写 * 

* 设置别名

```sql
SELECT 字段1 [AS 别名1]，字段2[AS 别名2]，··· FROM 表名;
```



* 去除重复记录

```SQL
SELECT DISINCT 字段列表 FROM 表名;
```



#### 条件查询

```sql
SELECT 字段列表 FROM 表名 WHERE 条件列表;
```

* 条件

![](D:/Code/md/some-note-md/img/mysql/%E6%9D%A1%E4%BB%B6%E6%9F%A5%E8%AF%A2.png)

* between ... and ... 
  * between 后面跟的是小的数字
  * and 后面是大的数，反过来则查找不到

* in

  * 没有in

  * ```sql
    select * from emp where age =18 or age = 20 or age = 40
    ```

  * in写法

  * ```SQL
    select * from emp where age in(18,20,40)
    ```

* like

  * 下划线按字符个数查询

    * 查询名字为2个字符的信息

  * ```sql
    select * from emp where name like '__'
    ```

  * 百分号匹配任意字符

    * 查询身份证末尾特定的'x'字符

  * ```sql
    select * from emp where name like '%x';
    ```

  * ```sql
    select * from emp where name like '_________________x'
    ```



#### 聚合函数

> 将一列数据作为一个整体，进行纵向计算

* **常见的聚合函数**

![image-20220729103136396](D:/Code/md/some-note-md/img/mysql/%E5%B8%B8%E8%A7%81%E7%9A%84%E8%81%9A%E5%90%88%E5%87%BD%E6%95%B0.png)

* 语法

```sql
SELECT 聚合函数（字段列表） FROM 表名;
```

* null 不参与聚合函数的计算



统计数量

```sql
select count(id) from emp;
```



#### 分组查询

**group by**

```sql
SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后的过滤条件]
```



where 和 having 的区别

* 执行机制不同：where是分组之前进行过滤，不满足where条件，不参与分组；
  * 而having是分组之后对结果进行过滤
* 判断条件不同：where不能对聚合函数进行判断，
  * 而having可以

**注意**

* 执行顺序：where > 聚合函数 > having
* 分组之后，查询的字段一般为聚合函数和分段函数，查询其他字段无任何意义



#### 排序查询

**order by**

```sql
SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1 , 字段2 排序方式2;
```



**排序方式**

* ASC 
  * 升序，（默认值）
* DESC
  * 降序



**多字段排序，当第一个字段值相同时候，才会根据第二个字段进行排序**



#### 分页查询

```sql
SELECT 字段列表 FROM 表名 LIMIT 起始索引，查询记录数;
```



**注意**

* 起始索引从 0 开始，起始索引 = （ 查询页码  -  1 ） *  每页显示记录数
* 分页查询是数据库的方言，不同的数据库中有不同的实现，MYSQL中是 LIMIT
* 如果查询的是第一页数据，起始索引可以省略，直接简写为 limit 10 （10是记录数）



##### 案例

* 查询年龄为20,21,22的女性员工信息

```sql
select * from emp where gender = '女' age in(20,21,22);
```

* 查询性别为男，并且年龄在 20 - 40(含)岁之间的姓名为三个字的员工

```sql
select * from emp where gender = '男' age and (age between 20 and 40) and name like '___';
```

* 统计员工表中，年龄小于60，男性员工和女性员工的人数

```sql
select gender , count(*) from emp where age <  60 group by gender
```

* 查询所有年龄小于35岁，员工的姓名和年龄，并对结果进行升序排序，如果年龄相同则按照入职时间降序排列

```sql
select name , age from emp where age <= 35 order by age , entrydate desc;
```

* 查询性别为男，且年龄在20-40岁（含） 内的前5个员工的信息，对查询结果按年龄升序排列，年龄相同则按入职时间进行升序排列

```sql
select * from emp where age between 20 and 40  order by age , entrydata limit 5;
```



#### 执行顺序

![image-20220731102149873](D:/Code/md/some-note-md/img/mysql/DQL%E7%9A%84%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F%E5%92%8C%E7%BC%96%E5%86%99%E9%A1%BA%E5%BA%8F.png)

![image-20220731102633429](D:/Code/md/some-note-md/img/mysql/DQL%E5%B0%8F%E7%BB%93.png)



### DCL

>  数据控制语言，用来管理数据库用户，控制数据库的访问权限

开发人员操作较为少，主要是 DBA（数据库管理员）使用

#### 管理用户



##### 查询用户

```sql
USE mysql;
SELECT * FROM user;
```

* 系统的用户信息储存在 mysql 表中
* 查询用户表就能够显示所有的用户信息



##### 创建用户

```sql
CREATE USER '用户名' @ '主机名' IDENTIFIED BY '密码';
```

* localhost  本机
* **% 任意主机**



##### 修改用户密码

```sql
ALTER USER '用户名' @'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
```



##### 删除用户

```sql
DROP USER '用户名' @'主机名';
```



#### 权限控制

![image-20220731113840108](D:/Code/md/some-note-md/img/mysql/%E6%9D%83%E9%99%90%E6%8E%A7%E5%88%B6.png)



##### 查询权限

```sql
SELECT GRANTS FOR '用户名' @'主机名';
```



##### 授予权限

```sql
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
```



##### 撤销权限

```SQL
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```



**注意**

* 多个权限直接使用逗号分割
* 授权时，数据库和表名可以使用 * 进行通配，代表所有



## 函数

#### 字符串函数

| **函  数**                     | **功  能**                                                   |
| ------------------------------ | ------------------------------------------------------------ |
| **CONCAT(str1,str2,...,strn)** | 将str1,str2,...,strn连接为一个完整的字符串                   |
| INSERT(str,x,y,instr)          | 将字符串str从第x开始，y个字符串长度的子串替换为字符串instr   |
| LOWER(str)                     | 将字符串str中的所有字母变成小写                              |
| UPPER(str)                     | 将字符串str中的所有字母变成大写                              |
| LEFT(str,x)                    | 返回字符串最左边的x个字符                                    |
| RIGHT(str,x)                   | 返回字符串最右边的x个字符                                    |
| **LPAD**(str,n,pad)            | 使用字符串pad对字符串str最**左边进行填充**，直到长度为n个字符长度 |
| **RPAD**(str,n,pad)            | 使用字符串pad对字符串str最**右边进行填充**，直到长度为n个字符长度 |
| **LTRIM**(str)                 | 去掉str左边的空格                                            |
| **RTRIM(str)**                 | 去掉str右边的空格                                            |
| REPEAT(str,x)                  | 返回字符串str重复x次的结果                                   |
| REPLACE(str,a,b)               | 使用字符串b替换字符串str中所有出现的字符串a                  |
| STRCMP(str1,str2)              | 比较字符串str1和str2                                         |
| **TRIM**(str)                  | 去掉字符串行头和行尾的空格                                   |
| **SUBSTRING**(str,x,y)         | 返回字符串str中从x位置起y个字符串长度的字符串                |

* 实例

```sql
select concat('Hello',' mysql');

//  Hello mysql
```

* 在工号前面补0

```sql
update emp set workno = lpad(workno,5,'0');
```



#### 数值函数

| **函  数**     | **功  能**                     |
| -------------- | ------------------------------ |
| ABS(x)         | 返回数值x的绝对值              |
| **CEIL**(x)    | 返回大于或等于x的最小整数值    |
| **FLOOR**(x)   | 返回小于或等于x的最大整数值    |
| **MOD**(x,y)   | 返回x除以y的余数               |
| **RAND**()     | 返回0~1内的随机数              |
| **ROUND**(x,y) | 返回x四舍五入后有y位小数的数值 |
| TRUNCATE(x,y)  | 返回数值x且截断为y位小数的数值 |

* 6位验证码

```sql
select lpad(round(rand()*1000000,0 ),6,0)
```



#### 日期函数

| **函  数**                        | **功  能**                                          |
| --------------------------------- | --------------------------------------------------- |
| **CURDATE**()                     | 获取当前日期                                        |
| **CURTIME**()                     | 获取当前时间                                        |
| **NOW**()                         | 获取当前的日期和时间                                |
| **DATEDIFF(date1,date2)**         | 返回起始时间**date1和结束时间**date2之间的天数      |
| UNIX_TIMESTAMP(date)              | 获取日期的UNIX时间戳                                |
| FROM_UNIXTIME()                   | 获取UNIX时间戳的日期值                              |
| WEEK(date)                        | 返回日期date为一年中的第几天                        |
| DAY(date)                         | 获取指定date的日期                                  |
| **YEAR**(date)                    | 返回日期date的年份                                  |
| HOUR(time)                        | 返回时间time的小时值                                |
| MINUTE(time)                      | 返回时间time的分钟值                                |
| MONTHNAME(date)                   | 返回时间date的月份                                  |
| DATE_ADD(data,INTERVAL expt type) | 返回一个日期/时间、值加上一个时间间隔expr后的时间值 |

```sql
select datediff(curdate(), '2022-08-26');
select date_add(now(), INTERVAL 70 DAY);
```



* 根据员工入职天数，并根据入职天数倒叙排列

```sql
select name, datediff(curdate(),entrydate) as 'entrydays' from emp order by entrydays desc;
```



#### 流程控制函数

| **函数**                                                   | **功能**                                        |
| ---------------------------------------------------------- | ----------------------------------------------- |
| IF(expr1,expr2,expr3)                                      | 如果expr1是真, 返回expr2, 否则返回expr3         |
| IFNULL(expr1,expr2)                                        | 如果expr1不是NULL,返回expr1,否则返回expr2       |
| CASE WHEN [value1] THEN[result1]… ELSE[default] END        | 如果value是真, 返回result1,否则返回default      |
| CASE [expr] WHEN [value1] THEN[result1]… ELSE[default] END | 如果expr等于value1, 返回result1,否则返回default |

*  查询员工的姓名和工作地址，（北京，上海---->一线城市，其他改为 二线城市

```sql
select
	name,
	(case workaddress when '北京' then '一线城市' when '上海' then '一线城市' else '二线城市' end) as '工作地址'
from emp;
```

* 根据成绩划分等级

```sql
select
    id,
    name,
    (case when math >= 85 then '优秀' when math >= 60 then '及格' else '不及格' end) '数学',
    (case when english >= 85 then '优秀' when english >= 60 then '及格' else '不及格' end) '英语',
    (case when chinese >= 85 then '优秀' when chinese >= 60 then '及格' else '不及格' end) '语文'
from score;
```

 

## 约束

> 作用于表中字段上的规则，用于限制存储在表中的数据
>
> 保证数据库中数据的正确性，有效性，完整性

![image-20220731175604196](D:/Code/md/some-note-md/img/mysql/%E7%BA%A6%E6%9D%9F.png)



**约束是作用于表中字段上的，可以在创建表/修改表的时候添加约束**

```sql
create table usrss(
    # 主键约束，自动增长
    id int primary key auto_increment comment '主键',
    # 非空且唯一
    name varchar(20) not null unique comment '姓名',
    # 检查年龄大于0且小于120
    age int check( age > 0 && age <= 120) comment '年龄',
    # 默认样式为1
    status char(1) default '1' comment '状态',
    gender char(1) comment '性别'
) comment '用户表';
```

#### 外键约束

> 让两张表的数据之间建立连接，从而保证数据的一致性和完整性

##### 添加外键

```sql
CREATE TABLE 表名(
	字段名 数据类型,
      [CONSTRAINT] [外键名称] ROREIGN KEY （外键字段名）REFERENCES 主表（主表列名）
);
```



````sql
ALTER TABLE 表名 ADD CONSTAAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表（主表列名）;
````



##### 删除外键

```sql
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
```



##### 删除更新行为

![image-20220731182826319](D:/Code/md/some-note-md/img/mysql/%E5%88%A0%E9%99%A4%E6%9B%B4%E6%96%B0%E8%A1%8C%E4%B8%BA.png)



```sql
ALTER TABLE 表名 ADD COSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名(主表字段名) ON UPDATE CASCADE ON DELETE CASCADE;
```

* on update 在更新时候的操作
* on delect 在删除的操作



## 多表查询

### 多表关系

> 表结构之间存在的各种联系

* 一对多（多对一）

案例：部门和员工的关系

实现：**在多的一方建立外键，指向一的一方的主键**



* 多对多

案例：学生与课程的关系

实现：**建立第三张中间表，中间表至少包含两个外键，分别关联两方主键**



* 一对一

案例：用户与用户详情的关系

实现：**在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的**



### 多表查询

#### 笛卡尔积

在多表查询的时候，能够通过选择多个表来查询信息，但是会产生很多的冗余

> 笛卡乘积是指在数学中，两个集合,A集合和B集合的所有组合情况
>
> **多表查询时。需要消除无效的笛卡尔积**

![image-20220731195617264](D:/Code/md/some-note-md/img/mysql/%E7%AC%9B%E5%8D%A1%E5%B0%94%E7%A7%AF.png)

* 消除

通过表直接的链接条件作为筛选条件

```sql
select * from emp , dept where emp.dept_id = dept.id;
```

* distinct
  * 去重	

#### 分类

连接查询

* 内连接
  * 查询A，B 交集部分的数据
* 外连接
  * 左外连接
    * 查询**左表**的所有数据，以及两张表交集部分的数据
    * 查询**右表**所有数据，以及两张表交集部分的数据

* 自连接
  * 当前表与自身的连接查询，自连接必须使用表别名
* 子查询

![image-20220731200820261](D:/Code/md/some-note-md/img/mysql/%E5%AD%90%E8%BF%9E%E6%8E%A5.png)



### 内连接

**内连接查询是查询两张表交集的部分**

* 隐式内连接

```sql
SELECT 字段列表 FROM 表1，表2 WHERE 条件 ... ;
```

```sql
-- 1. 查询每一个员工的姓名 , 及关联的部门的名称 (隐式内连接实现)
-- 表结构: emp , dept
-- 连接条件: emp.dept_id = dept.id
select emp.name , dept.name from emp , dept where emp.dept_id = dept.id ;
```

通常在多表查询中会给表起别名，但是**有了别名之后的表就不能通过原来的名字访问**



* 显式内连接

```sql
SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件;
```

```sql
-- 2. 查询每一个员工的姓名 , 及关联的部门的名称 (显式内连接实现)  --- INNER JOIN ... ON ...
-- 表结构: emp , dept
-- 连接条件: emp.dept_id = dept.id

select e.name, d.name from emp e inner join dept d  on e.dept_id = d.id;
```



### 外连接

* 左外连接

**查询表1（左表）的所有数据包含表1和表2交集部分的数据**

```sql
SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件...；
```

```sql
-- 1. 查询emp表的所有数据, 和对应的部门信息(左外连接)
-- 表结构: emp, dept
-- 连接条件: emp.dept_id = dept.id

select e.*, d.name from emp e left outer join dept d on e.dept_id = d.id;

select e.*, d.name from emp e left join dept d on e.dept_id = d.id;
```



* 右外连接

**查询表2（右表）的所有数据包含表1和表2交集部分的数据**

```sql
SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件...;
```

```sql
-- 2. 查询dept表的所有数据, 和对应的员工信息(右外连接)

select d.*, e.* from emp e right outer join dept d on e.dept_id = d.id;

-- 左外和右外交换，只需要更改表的顺序
select d.*, e.* from dept d left outer join emp e on e.dept_id = d.id;
```



### 自连接

**自连接查询，可以是内连接查询，也可以是外连接查询**

```sql
SELECT 字段列表 FROM 表a 别名a JOIN 表a 别名b ON 条件...;
```

```sql
-- 1. 查询员工 及其 所属领导的名字
-- 表结构: emp

select a.name , b.name from emp a , emp b where a.managerid = b.id;

-- 2. 查询所有员工 emp 及其领导的名字 emp , 如果员工没有领导, 也需要查询出来
-- 表结构: emp a , emp b

select a.name '员工', b.name '领导' from emp a left join emp b on a.managerid = b.id;
```



#### 联合查询

**union,union all**



> 对于 union 查询，就是把多次查询的结果合并起来，形成一个新的查询结果集

```sql
SELECT 字段列表 FROM 表a ...
UNION [ALL]
SELECT 字段列表 FROM 表b...;
```

* union all
  * 所有的结果都显示

* union 
  * 查询的结果去重

* **联合查询的多张表的列数必须保持一致，字段类型也需要保持一致**



### 子查询

> SQL语句中嵌套`SELECT`语句，称为**嵌套查询**，也叫作**子查询**

**子查询外部的语句可以是`insert,update,delete,select`的任意一个**

```SQL
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2);
```



* 根据子查询结果不同，分为
  * 标量子查询，（子查询结果为单个值）
  * 列子查询（子查询结果为一列）
  * 行子查询（子查询结果为一行）
  * 表子查询 （查询结果为多行多列）



* 根据子查询位置分为

  * where之后
  * from之后
  * select之后




#### 标量子查询

> 子查询的结果是单个值（数组，字符串，日期等），最简单的形式，这种子查询是标量子查询

常用操作符：`=, <> , > , >= , < , <=`

```sql
-- 1. 查询 "销售部" 的所有员工信息
-- a. 查询 "销售部" 部门ID
select id from dept where name = '销售部';

-- b. 根据销售部部门ID, 查询员工信息
select * from emp where dept_id = (select id from dept where name = '销售部');


-- 2. 查询在 "方东白" 入职之后的员工信息
-- a. 查询 方东白 的入职日期
select entrydate from emp where name = '方东白';

-- b. 查询指定入职日期之后入职的员工信息
select * from emp where entrydate > (select entrydate from emp where name = '方东白');
```



#### 列子查询

> 子查询的结果是一列（可以是多行）

常用操作符：`in, not in, any, some, all`

| **操作符** | **描述**                                    |
| ---------- | ------------------------------------------- |
| IN         | 在指定的集合范围之内，多选一                |
| NOT IN     | 不在指定的集合范围之内                      |
| ANY        | 子查询返回列表中，有任意一个满足即可        |
| SOME       | 与 any 等同，使用 some 的地方都可以使用 any |
| ALL        | 子查询返回列表的所有值都必须满足            |

```sql
- 1. 查询 "销售部" 和 "市场部" 的所有员工信息
-- a. 查询 "销售部" 和 "市场部" 的部门ID
select id from dept where name = '销售部' or name = '市场部';

-- b. 根据部门ID, 查询员工信息
select * from emp where dept_id in (select id from dept where name = '销售部' or name = '市场部');


-- 2. 查询比 财务部 所有人工资都高的员工信息
-- a. 查询所有 财务部 人员工资
select id from dept where name = '财务部';

select salary from emp where dept_id = (select id from dept where name = '财务部');

-- b. 比 财务部 所有人工资都高的员工信息
select * from emp where salary > all ( select salary from emp where dept_id = (select id from dept where name = '财务部') );


-- 3. 查询比研发部其中任意一人工资高的员工信息
-- a. 查询研发部所有人工资
select salary from emp where dept_id = (select id from dept where name = '研发部');

-- b. 比研发部其中任意一人工资高的员工信息
select * from emp where salary > some ( select salary from emp where dept_id = (select id from dept where name = '研发部') );
```



#### 行子查询

> 子查询返回的结果是一行（可以是多列）

常用操作符：`=, <>, IN, NOT, IN`

```sql
-- 1. 查询与 "张无忌" 的薪资及直属领导相同的员工信息 ;
-- a. 查询 "张无忌" 的薪资及直属领导
select salary, managerid from emp where name = '张无忌';

-- b. 查询与 "张无忌" 的薪资及直属领导相同的员工信息 ;
select * from emp where (salary,managerid) = (select salary, managerid from emp where name = '张无忌');
```



#### 表子查询

> 子查询的结果是多行多列

常用操作符：`IN`

```sql
-- 1. 查询与 "鹿杖客" , "宋远桥" 的职位和薪资相同的员工信息
-- a. 查询 "鹿杖客" , "宋远桥" 的职位和薪资
select job, salary from emp where name = '鹿杖客' or name = '宋远桥';

-- b. 查询与 "鹿杖客" , "宋远桥" 的职位和薪资相同的员工信息
select * from emp where (job,salary) in ( select job, salary from emp where name = '鹿杖客' or name = '宋远桥' );


-- 2. 查询入职日期是 "2006-01-01" 之后的员工信息 , 及其部门信息
-- a. 入职日期是 "2006-01-01" 之后的员工信息
select * from emp where entrydate > '2006-01-01';

-- b. 查询这部分员工, 对应的部门信息;
select e.*, d.* from (select * from emp where entrydate > '2006-01-01') e left join dept d on e.dept_id = d.id ;
```







##### 案例

```sql
-- 1. 查询员工的姓名、年龄、职位、部门信息 （隐式内连接）
-- 表: emp , dept
-- 连接条件: emp.dept_id = dept.id

select e.name , e.age , e.job , d.name from emp e , dept d where e.dept_id = d.id;


-- 2. 查询年龄小于30岁的员工的姓名、年龄、职位、部门信息（显式内连接）
-- 表: emp , dept
-- 连接条件: emp.dept_id = dept.id

select e.name , e.age , e.job , d.name from emp e inner join dept d on e.dept_id = d.id where e.age < 30;


-- 3. 查询拥有员工的部门ID、部门名称
-- 表: emp , dept
-- 连接条件: emp.dept_id = dept.id

select distinct d.id , d.name from emp e , dept d where e.dept_id = d.id;



-- 4. 查询所有年龄大于40岁的员工, 及其归属的部门名称; 如果员工没有分配部门, 也需要展示出来
-- 表: emp , dept
-- 连接条件: emp.dept_id = dept.id
-- 外连接

select e.*, d.name from emp e left join dept d on e.dept_id = d.id where e.age > 40 ;


-- 5. 查询所有员工的工资等级
-- 表: emp , salgrade
-- 连接条件 : emp.salary >= salgrade.losal and emp.salary <= salgrade.hisal

select e.* , s.grade , s.losal, s.hisal from emp e , salgrade s where e.salary >= s.losal and e.salary <= s.hisal;

select e.* , s.grade , s.losal, s.hisal from emp e , salgrade s where e.salary between s.losal and s.hisal;


-- 6. 查询 "研发部" 所有员工的信息及 工资等级
-- 表: emp , salgrade , dept
-- 连接条件 : emp.salary between salgrade.losal and salgrade.hisal , emp.dept_id = dept.id
-- 查询条件 : dept.name = '研发部'

select e.* , s.grade from emp e , dept d , salgrade s where e.dept_id = d.id and ( e.salary between s.losal and s.hisal ) and d.name = '研发部';



-- 7. 查询 "研发部" 员工的平均工资
-- 表: emp , dept
-- 连接条件 :  emp.dept_id = dept.id

select avg(e.salary) from emp e, dept d where e.dept_id = d.id and d.name = '研发部';



-- 8. 查询工资比 "灭绝" 高的员工信息。
-- a. 查询 "灭绝" 的薪资
select salary from emp where name = '灭绝';

-- b. 查询比她工资高的员工数据
select * from emp where salary > ( select salary from emp where name = '灭绝' );


-- 9. 查询比平均薪资高的员工信息
-- a. 查询员工的平均薪资
select avg(salary) from emp;

-- b. 查询比平均薪资高的员工信息
select * from emp where salary > ( select avg(salary) from emp );





-- 10. 查询低于本部门平均工资的员工信息

-- a. 查询指定部门平均薪资  1
select avg(e1.salary) from emp e1 where e1.dept_id = 1;
select avg(e1.salary) from emp e1 where e1.dept_id = 2;

-- b. 查询低于本部门平均工资的员工信息
select * from emp e2 where e2.salary < ( select avg(e1.salary) from emp e1 where e1.dept_id = e2.dept_id );


-- 11. 查询所有的部门信息, 并统计部门的员工人数
select d.id, d.name , ( select count(*) from emp e where e.dept_id = d.id ) '人数' from dept d;

select count(*) from emp where dept_id = 1;


-- 12. 查询所有学生的选课情况, 展示出学生名称, 学号, 课程名称
-- 表: student , course , student_course
-- 连接条件: student.id = student_course.studentid , course.id = student_course.courseid

select s.name , s.no , c.name from student s , student_course sc , course c where s.id = sc.studentid and sc.courseid = c.id ;



```





## 事务

> 一组操作的集合，他是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或者撤销操作请求，这些操作**要么同时成功，要么同时失败。**

**开启事务---> (如果抛出异常，则回滚事务) ----> 提交事务**

默认 MYSQL的事务是自动提交的，也就是说，当执行一条 DML 语句，MySQL就会立即隐式的提交事务



* 案例

张三向李四转账，先要查询张三的账户，再从张三的账户中扣掉1000块，然后李四的账户增加1000块。如果中间的扣款步骤产生异常，而李四的账户去没收到钱就发生错误。



### 事务操作

#### 查看/设置事务提交方式

```sql
SELECT @@autocommit;
SET @@autocommit = 0;
```

* 1，代表自动提交
* 0，为手动提交

**改为手动提交时，但前窗口的指令执行都不会有任何修改，都需要执行一次`commit`命令**



#### 提交事务

```sql
COMMIT;
```



#### 回滚事务

```sql
ROLLBACK;
```



##### 案例

```sql
-- ---------------------------- 事务操作 ----------------------------
-- 数据准备
create table account(
    id int auto_increment primary key comment '主键ID',
    name varchar(10) comment '姓名',
    money int comment '余额'
) comment '账户表';
insert into account(id, name, money) VALUES (null,'张三',2000),(null,'李四',2000);


-- 恢复数据
update account set money = 2000 where name = '张三' or name = '李四';
```

```sql
select @@autocommit;

set @@autocommit = 0; -- 设置为手动提交

-- 转账操作 (张三给李四转账1000)
-- 1. 查询张三账户余额
select * from account where name = '张三';

-- 2. 将张三账户余额-1000
update account set money = money - 1000 where name = '张三';

# 程序执行报错 ...

-- 3. 将李四账户余额+1000
update account set money = money + 1000 where name = '李四';


-- 提交事务
commit;

-- 回滚事务
rollback ;
```



#### 开启事务

```sql
SELECT TRANSACTION 或 BEGIN;
```



#### 提交事务

```sql
COMMIT;
```



#### 回滚事务

```sql
ROLLBACK;
```



```sql
-- 方式二
-- 转账操作 (张三给李四转账1000)
start transaction ;

-- 1. 查询张三账户余额
select * from account where name = '张三';

-- 2. 将张三账户余额-1000
update account set money = money - 1000 where name = '张三';

程序执行报错 ...

-- 3. 将李四账户余额+1000
update account set money = money + 1000 where name = '李四';


-- 提交事务
commit;

-- 回滚事务
rollback;

```





### 事务的四大特性

* 原子性

> 事务是不可分割的最小操作单元，要么全部成功，要么全部失败



* 一致性

> 事务完成时，必须使用所有的数据都保持一致状态



* 隔离性

> 数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行



* 持久性

> 事务一旦提交成功或者回滚，它对数据库中的数据的改变就是永久的



### 并发事务问题

| 问题       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| 脏读       | 一个事务读到另一个事务还没有提交的数据                       |
| 不可重复读 | 一个事务先后读取同一条记录，但两次读取的数据不同，           |
| 幻读       | 一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在 |



### 事务的隔离级别

> 解决事务并发的事务问题

| 事务隔离级别                      | 脏读 | 不可重复读 | 幻读 |
| --------------------------------- | ---- | ---------- | ---- |
| 读未提交（read-uncommitted）      | 是   | 是         | 是   |
| 不可重复读（read-committed）      | 否   | 是         | 是   |
| 可重复读（repeatable-read）(默认) | 否   | 否         | 是   |
| 串行化（serializable）            | 否   | 否         | 否   |



#### 查看和设置

* 查看事务的隔离级别

```SQL
SELECT @@TRANSACTION_ISOLATION;
```

* 设置事务的隔离级别

```sql
SET [SESSIION | GLOBAL ] TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE};
```



```sql
-- 查看事务隔离级别
select @@transaction_isolation;

-- 设置事务隔离级别
set session transaction isolation level read uncommitted ;

set session transaction isolation level repeatable read ;
```

