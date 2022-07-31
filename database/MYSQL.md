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

