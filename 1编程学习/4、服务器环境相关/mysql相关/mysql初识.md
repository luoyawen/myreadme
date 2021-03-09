# mysql-day01

> 什么是数据库

数据库（**DataBa**，简称DB）

长期存放在计算机内，有组织，可共享的大量数据的集合，是一个数据“仓库”。

> 数据库分类

- 关系型数据库（SQL）

  MySQL，Oracle，Sql server，SQlite，DB2，…

  关系型数据库通过外键来关联建立表与表之间的关系

- 非关系型数据库（NOSQL）

  Redis，MongoDB,…

  非关系型数据库通常是指数据以对象的形式存储在数据库中

  > 什么是DBMS

数据库管理系统（DataBase Management System）

是数据库管理软件，科学组织和存储数据，高效获取和维护数据

Mysql 就是一个数据库管理系统

### mysql

概念：是现在流向的开源的，免费的，关系型数据库

由Mysql AB 公司开发，目前属于Oracle旗下产品。

**特点**

- 免费，开源的数据库
- 小巧功能齐全
- 使用便捷
- 可以运行与Windows或linux操作系统之上
- 适用于中小型、大型网站应用

##### mysql启动

```
net stop mysqlnet start mysql
```

##### mysql连接

```
mysql -uroot -p12346
```

##### 基本的几个数据库命令

```
update user set password=password('123456')where user='root'; 修改密码flush privileges; 刷新数据库show databases; 显示所有数据库use dbname；打开某个数据库show tables; 显示数据库mysql中所有的表describe user; 显示表mysql数据库中user表的列信息create database name; 创建数据库use databasename; 选择数据库exit; 退出Mysql? 命令关键词 : 寻求帮助-- 表示注释
```

### 数据库操作

**结构化查询语句分类**

| 名称                | 解释                                     | 命令                    |
| ------------------- | ---------------------------------------- | ----------------------- |
| DDL（数据定义语言） | 定义和管理数据对象，如数据库，数据表     | CREATE、DROP、ALTER     |
| DML(数据操作语言)   | 用于操作数据库对象中包含的数据           | INSERT、UPDATE、DELETE  |
| DQL（数据查询语言） | 用于查询数据库数据                       | SELECT                  |
| DCL（数据控制语言） | 用于管理数据库语言，包括管理权限数据更改 | GRANT、commit、rollback |

**SQLyog工具**

基字符集：utf8

数据库排序规则：utf8_general_ci 校对不区分大小写

**创建数据库**：create database [if not exists] 数据库名；

**删除数据库**：drop database [if exists] 数据库名；

**查看数据库**：show database;

**使用数据库**：use 数据库名

#### 创建数据表

```
create table [if not exists] `表名`(    '字段1' 类型 [属性][索引][注释],    '字段2' 类型 [属性][索引][注释],    '字段3' 类型 [属性][索引][注释],    ...)
```

**说明 :** 反引号用于区别MySQL保留字与普通字符而引入的 (键盘esc下面的键).

```
-- 目标 : 创建一个school数据库-- 创建学生表(列,字段)-- 学号int 登录密码varchar(20) 姓名,性别varchar(2),出生日期(datatime),家庭住址,email-- 创建表之前 , 一定要先选择数据库CREATE TABLE IF NOT EXISTS `student` (`id` int(4) NOT NULL AUTO_INCREMENT COMMENT '学号',`name` varchar(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',`pwd` varchar(20) NOT NULL DEFAULT '123456' COMMENT '密码',`sex` varchar(2) NOT NULL DEFAULT '男' COMMENT '性别',`birthday` datetime DEFAULT NULL COMMENT '生日',`address` varchar(100) DEFAULT NULL COMMENT '地址',`email` varchar(50) DEFAULT NULL COMMENT '邮箱',PRIMARY KEY (`id`)) ENGINE=InnoDB DEFAULT CHARSET=utf8-- 查看数据库的定义SHOW CREATE DATABASE school; --查看创建数据库的语句-- 查看数据表的定义SHOW CREATE TABLE student; -- 查看student数据表的定义语句-- 显示表结构DESC student;  -- 设置严格检查模式(不能容错了)SET sql_mode='STRICT_TRANS_TABLES';
```

#### 数据表的类型

> 设置数据表的类型

```
CREATE TABLE 表名(   -- 省略一些代码   -- Mysql注释   -- 1. # 单行注释   -- 2. /*...*/ 多行注释)ENGINE = MyISAM (or InnoDB)-- 查看mysql所支持的引擎类型 (表类型)SHOW ENGINES;
```

**mysql的数据表类型：** MyISAM , InnoDB , HEAP , BOB , CSV等…

常见的 MyISAM 与 InnoDB 类型：

**( 适用场合 )**:

- 适用 MyISAM : 节约空间及相应速度

- 适用 InnoDB : 安全性 高, 事务处理及多用户操作数据表

- | | MYISAM | INNODB |
  | :—————- | ——— | ——————— |
  | 事务支持 | 不支持 | 支持 |
  | 数据行锁定 | 不支持 | 支持 |
  | 外键约束 | 不支持 | 支持 |
  | 全文索引 | 支持 | 不支持 |
  | 表空间的大小 | 较小 | 较大，约为两倍 |

  > 设置数据库表的字符集编码

  ```
  default charset=utf8
  ```

#### 修改数据库

**修改表名** :

```
ALTER TABLE 旧表名 RENAME AS 新表名
```

**添加表字段** :

```
 ALTER TABLE 表名 ADD字段名 列属性[属性]
```

**修改表字段** :（修改约束，重命名）

```
ALTER TABLE 表名 MODIFY 字段名 列类型[属性]    -- 修改约束ALTER TABLE 表名 CHANGE 旧字段名 新字段名 列属性[属性] -- 字段重命名
```

**删除表字段** :

```
ALTER TABLE 表名 DROP 字段名
```

**删除数据表**：

```
DROP TABLE [IF NOT EXISTS] 表名* IF NOT EXISTS为可选 , 判断是否存在该数据表* 如删除不存在的数据表会抛出错误
```

==所有的创建和删除操作尽量加上判断（if exists），以免报错==

**注释**：

> 单行注释 # 注释内容
> 多行注释 / *注释内容* /
> 单行注释 — 注释内容 (标准SQL注释风格，要求双破折号后加一空格符（空格、TAB、换行等）)
>
> **可用反引号（`）为标识符（库名、表名、字段名、索引、别名）包裹，以避免与关键字重名！中文也可以作为标识符**
>
> **sql关键字大小写不敏感，建议用小写，这样容易识别！**

**模糊通配符**

> _ 任意单个字符
> % 任意多个字符，甚至包括零字符
> 单引号需要进行转义 \ \’

### MySQL数据管理

### 外键

> 外键概念

如果公共关键字在一个关系中是主关键字，那么这个公共关键字被称为另一个关系的外键。由此可见，外键表示了两个关系之间的相关联系。以另一个关系的外键作主关键字的表被称为**主表**，具有此外键的表被称为主表的**从表**。

在实际操作中，将一个表的值放入第二个表来表示关联，所使用的值是第一个表的主键值(在必要时可包括复合主键值)。此时，第二个表中保存这些值的属性称为外键(**foreign key**)。

**外键作用**

保持数据**一致性**，**完整性**，主要目的是控制存储在外键表中的数据,**约束**。使两张表形成关联，外键只能引用外表中的列的值或使用空值。

> 创建外键

**方式一：建表时指定外键约束**

```
-- 创建外键的方式一 : 创建子表同时创建外键-- 年级表 (id\年级名称)CREATE TABLE `grade` (`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级ID',`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',PRIMARY KEY (`gradeid`)) ENGINE=INNODB DEFAULT CHARSET=utf8-- 学生信息表 (学号,姓名,性别,年级,手机,地址,出生日期,邮箱,身份证号)CREATE TABLE `student` (`studentno` INT(4) NOT NULL COMMENT '学号',`studentname` VARCHAR(20) NOT NULL DEFAULT '匿名' COMMENT '姓名',`sex` TINYINT(1) DEFAULT '1' COMMENT '性别',`gradeid` INT(10) DEFAULT NULL COMMENT '年级',`phoneNum` VARCHAR(50) NOT NULL COMMENT '手机',`address` VARCHAR(255) DEFAULT NULL COMMENT '地址',`borndate` DATETIME DEFAULT NULL COMMENT '生日',`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',`idCard` VARCHAR(18) DEFAULT NULL COMMENT '身份证号',PRIMARY KEY (`studentno`),KEY `FK_gradeid` (`gradeid`),CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`)) ENGINE=INNODB DEFAULT CHARSET=utf8
```

**方式二:建表后修改**

```
-- 创建外键方式二 : 创建子表完毕后,修改子表添加外键ALTER TABLE `student`ADD CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`);-- alter table 表 add constraint 约束名 foreign key(作为外键的列) references 那个表（那个字段）
```

> **删除外键**

操作：删除 grade 表，发现报错

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-wFQIXRJk-1607570778670)(data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)]

**注意** : 删除具有主外键关系的表时 , 要先删子表 , 后删主表

```
-- 删除外键ALTER TABLE student DROP FOREIGN KEY FK_gradeid;-- 发现执行完上面的,索引还在,所以还要删除索引-- 注:这个索引是建立外键的时候默认生成的ALTER TABLE student DROP INDEX FK_gradeid;
```

#### 外键的优点

> - 保证数据的完整性和一致性
> - 级联操作方便
> - 将数据的完整性判断托付给数据库完成，减少了程序的代码量

以上的操作都是物理外键。数据库级别的外键。我们不建议使用！（避免数据库过多造成的困扰，这里了解即可）**

==最佳实践==：

- 数据库就是单纯的表，只用来存储数据，只有行和列，数据和字段
- 要想使用多张表的数据，想使用外键的话（通过程序去实现）

### 为什么不使用外键

- 阿里的java规范里面有一条：【强制】不得使用外键与级联，一切外键概念必须在应用层解决。

  **原因**：每次做delete或者update都必须考虑外键约束，会导致开发的时候很痛苦，测试数据极不方便。