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

#### 查询

* 查询所有数据库

```SQL 
SHOW DATABASES;
```

* 查询当前数据库

```SQL
SELECT DATABASE();
```

需要添加括号

#### 创建

```SQL
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];
```



#### 删除

```SQL 
DROP DATABASE [IF EXISTS] 数据库名;
```



#### 使用

```sql
USE 数据库名;
```

