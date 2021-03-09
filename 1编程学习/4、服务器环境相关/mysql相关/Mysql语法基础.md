# mysql語法精要

## 一 DDL 創建數據庫

```
-- 直接创建数据库 db1create database db1;-- 判断是否存在，如果不存在则创建数据库 db2create database if not exists db2;-- 创建数据库并指定字符集为 gbkcreate database db3 default character set gbk;-- 查看所有的数据库show databases;-- 查看某个数据库的定义信息show create database db3;show create database db1;
```

## 二 DDL操作表結構

```
create table student (    id int, -- 整数    name varchar(20), -- 字符串    birthday date -- 生日，最后没有逗号);use day21;show tables;desc student;show create table student;CREATE TABLE `student` (`id` int(11) DEFAULT NULL,`name` varchar(20) DEFAULT NULL,`birthday` date DEFAULT NULL) ENGINE=InnoDB DEFAULT CHARSET=utf8;-- 创建一个 s1 的表与 student 结构相同create table s1 like student;desc s1;-- 直接删除表 s1 表drop table s1;-- 判断表是否存在并删除 s1 表drop table if exists `create`;alter table student modify remark varchar(100);alter table student change remark intro varchar(30);rename table student to student2;alter table student2 character set gbk;
```

## 三 DML操作表中的數據

### 插入:

語法:

INSERT INTO 表名 (字段名 1, 字段名 2, 字段名 3…) VALUES (值 1, 值 2, 值 3);

INSERT INTO 表名 VALUES (值 1, 值 2, 值 3…);

INSERT INTO 表名 (字段名 1, 字段名 2, …) VALUES (值 1, 值 2, …);

```
insert into student (id,name,age,sex) values (1, '孙悟空', 20, '男');insert into student (id,name,age,sex) values (2, '孙悟天', 16, '男');-- 插入所有列insert into student values (3, '孙悟饭', 18, '男', '龟仙人洞中');-- 如果只插入部分列，必须写列名insert into student values (3, '孙悟饭', 18, '男');select * from student;
```

### 更新:

語法:

UPDATE 表名 SET 列名=值 [WHERE 条件表达式]

UPDATE: 需要更新的表名
SET: 修改的列值
WHERE: 符合条件的记录才更新
你可以同时更新一个或多个字段。
你可以在 WHERE 子句中指定任何条件。

```
-- 不带条件修改数据，将所有的性别改成女update student set sex = '女';-- 带条件修改数据，将 id 号为 2 的学生性别改成男update student set sex='男' where id=2;-- 一次修改多个列，把 id 为 3 的学生，年龄改成 26 岁，address 改成北京update student set age=26, address='北京' where id=3;
```

### 刪除表記錄:

DELETE FROM 表名 [WHERE 条件表达式]
如果没有指定 WHERE 子句，MySQL 表中的所有记录将被删除。
你可以在 WHERE 子句中指定任何条件

不带条件删除数据:

DELETE FROM 表名;

带条件删除数据

DELETE FROM 表名 WHERE 字段名=值;

使用 truncate 删除表中所有记录

TRUNCATE TABLE 表名;

truncate 相当于删除表的结构，再创建一张表。

```
-- 带条件删除数据，删除 id 为 1 的记录delete from student where id=1;-- 不带条件删除数据,删除表中的所有数据delete from student;
```

## 四 DQL查詢表數據

查询不会对数据库中的数据进行修改.只是一种显示数据的方式

SELECT 列名 FROM 表名 [WHERE 条件表达式]
1) SELECT 命令可以读取一行或者多行记录。
2) 你可以使用星号（*）来代替其他字段，SELECT 语句会返回表的所有字段数据
3) 你可以使用 WHERE 语句来包含任何条件。

語法:

select
字段列表
from
表名列表
where
条件列表
group by
分组字段
having
分组之后的条件
order by
排序
limit
分页限定

```
select * from student;-- 给所有的数学加 5 分select math+5 from student;-- 查询 math + english 的和select * from student;select *,(math+english) as 总成绩 from student;-- as 可以省略select *,(math+english) 总成绩 from student;
```

### 簡單的條件查詢

SELECT 字段名 FROM 表名 WHERE 条件;
流程：取出表中的每条数据，满足条件的记录就返回，不满足条件的记录不返回

準備數據

```
-- 创建一个学生表，包含如下列：CREATE TABLE student3 (id int, -- 编号name varchar(20), -- 姓名age int, -- 年龄sex varchar(5), -- 性别address varchar(100), -- 地址math int, -- 数学english int -- 英语);INSERT INTO student3(id,NAME,age,sex,address,math,english) VALUES (1,'马云',55,'男','杭州',66,78),(2,'马化腾',45,'女','深圳',98,87),(3,'马景涛',55,'男','香港',56,77),(4,'柳岩',20,'女','湖南',76,65),(5,'柳青',20,'男','湖南',86,NULL),(6,'刘德华',57,'男','香港',99,99),(7,'马德',22,'女','香港',99,99),(8,'德玛西亚',18,'男','南京',56,65);
```

運算符:

| 比較運算符          | 說明                                                         |
| ------------------- | ------------------------------------------------------------ |
| >、<、<=、>=、=、<> | <>在 SQL 中表示不等于，在 mysql 中也可以使用!= 没有==        |
| BETWEEN…AND         | 在一个范围之内，如：between 100 and 200相当于条件在 100 到 200 之间，包头又包尾 |
| IN(集合)            | 集合表示多个值，使用逗号分隔                                 |
| LIKE ‘张%’          | 模糊查询                                                     |
| IS NULL             | 查询某一列为 NULL 的值，注：不能写=NULL                      |
|                     |                                                              |

```
-- 查询 math 分数大于 80 分的学生select * from student3 where math>80;-- 查询 english 分数小于或等于 80 分的学生select * from student3 where english <=80;-- 查询 age 等于 20 岁的学生select * from student3 where age = 20;-- 查询 age 不等于 20 岁的学生，注：不等于有两种写法select * from student3 where age <> 20;select * from student3 where age != 20;
```

邏輯運算符

| 邏輯運算符 | 說明                                   |      |      |
| ---------- | -------------------------------------- | ---- | ---- |
| and 或&&   | 与，SQL 中建议使用前者，后者并不通用。 |      |      |
| or 或\     | \                                      |      | 或   |
| not 或 !   | 非                                     |      |      |

```
-- 查询 age 大于 35 且性别为男的学生(两个条件同时满足)select * from student3 where age>35 and sex='男';-- 查询 age 大于 35 或性别为男的学生(两个条件其中一个满足)select * from student3 where age>35 or sex='男';-- 查询 id 是 1 或 3 或 5 的学生select * from student3 where id=1 or id=3 or id=5;
```

in 關鍵字

SELECT 字段名 FROM 表名 WHERE 字段 in (数据 1, 数据 2…);
in 里面的每个数据都会作为一次条件，只要满足条件的就会显示

```
-- 查询 id 是 1 或 3 或 5 的学生select * from student3 where id in(1,3,5);-- 查询 id 不是 1 或 3 或 5 的学生select * from student3 where id not in(1,3,5);
```

範圍查詢

BETWEEN 值 1 AND 值 2
表示从值 1 到值 2 范围，包头又包尾
比如：age BETWEEN 80 AND 100 相当于： age>=80 && age<=100

```
-- 查询 english 成绩大于等于 75，且小于等于 90 的学生select * from student3 where english between 75 and 90;
```

like 關鍵字

LIKE 表示模糊查询
SELECT * FROM 表名 WHERE 字段名 LIKE ‘通配符字符串’;

mysql 通配符

| 通配符 | 說明               |
| ------ | ------------------ |
| %      | 匹配任意多个字符串 |
| _      | 匹配一个字符       |

```
-- 查询姓马的学生select * from student3 where name like '马%';select * from student3 where name like '马';-- 查询姓名中包含'德'字的学生select * from student3 where name like '%德%';-- 查询姓马，且姓名有两个字的学生select * from student3 where name like '马_';
```

## 五 mysql表的約束與數據庫設計

學習目標:

1. 能夠使用sql 語句進行排序
2. 能夠使用聚合函數
3. 能夠使用sql語句進行分組查詢
4. 能夠完成數據的備份和恢復
5. 能夠使用sql語句添加主鍵,外鍵,唯一,非空約束
6. 能夠說出多表之間的關係以及建表原則
7. 能夠理解三大範式

### DQL查詢語句

#### 排序:

通过 ORDER BY 子句，可以将查询出的结果进行排序(排序只是显示方式，不会影响数据库中数据的顺序)

SELECT 字段名 FROM 表名 WHERE 字段=值 ORDER BY 字段名 [ASC|DESC];
ASC: 升序，默认值
DESC: 降序

單列排序

只按照某一個字段進行排序,單列排序

```
-- 查询所有数据,使用年龄降序排序select * from student order by age desc;
```

組合排序

同时对多个字段进行排序，如果第 1 个字段相等，则按第 2 个字段排序，依次类推。

語法:

SELECT 字段名 FROM 表名 WHERE 字段=值 ORDER BY 字段名 1 [ASC|DESC], 字段名 2 [ASC|DESC];

```
-- 查询所有数据,在年龄降序排序的基础上，如果年龄相同再以数学成绩升序排序select * from student order by age desc, math asc;
```

#### 聚合函數

之前我们做的查询都是横向查询，它们都是根据条件一行一行的进行判断，而使用聚合函数查询是纵向查询，
它是对一列的值进行计算，然后返回一个结果值。聚合函数会忽略空值 NULL。

| sql中的聚合函數 | 作用                   |
| --------------- | ---------------------- |
| max(列名)       | 求这一列的最大值       |
| min(列名)       | 求这一列的最小值       |
| avg(列名)       | 求这一列的平均值       |
| count(列名)     | 统计这一列有多少条记录 |
| sum(列名)       | 对这一列求总和         |

語法:

SELECT 聚合函数(列名) FROM 表名;

```
-- 查询学生总数select count(id) as 总人数 from student;select count(*) as 总人数 from student;
```

我们发现对于 NULL 的记录不会统计，建议如果统计个数则不要使用有可能为 null 的列，但如果需要把 NULL
也统计进去呢？

IFNULL(列名，默认值)
如果列名不为空，返回这列的值。如果为 NULL，则返回默认值。

— 查询 id 字段，如果为 null，则使用 0 代替
select ifnull(id,0) from student;

select count(ifnull(id,0)) from student;

```
-- 查询年龄大于 20 的总数select count(*) from student where age>20;-- 查询数学成绩总分select sum(math) 总分 from student;-- 查询数学成绩平均分select avg(math) 平均分 from student;-- 查询数学成绩最高分select max(math) 最高分 from student;-- 查询数学成绩最低分select min(math) 最低分 from student;
```

#### 分組

分组查询是指使用 GROUP BY 语句对查询信息进行分组，相同数据作为一组
SELECT 字段 1,字段 2… FROM 表名 GROUP BY 分组字段 [HAVING 条件];

GROUP BY 怎么分组的？

将分组字段结果中相同内容作为一组，如按性别将学生分成 2 组。

GROUP BY 将分组字段结果中相同内容作为一组，并且返回每组的第一条数据，所以单独分组没什么用处。
分组的目的就是为了统计，一般分组会跟聚合函数一起使用。

```
-- 按性别进行分组，求男生和女生数学的平均分select sex, avg(math) from student3 group by sex;
```

**注意：当我们使用某个字段分组,在查询的时候也需要将这个字段查询出来,否则看不到数据属于哪组的**

查詢男女有多少人

1) 查詢所有數據,按性別分組

2) 統計每組人數

```
select sex, count(*) from student3 group by sex;
```

查询年龄大于 25 岁的人,按性别分组,统计每组的人数

1) 先過濾掉年齡小於25歲的人

2) 再分組

3) 最後統計每組的人數

```
select sex, count(*) from student3 where age > 25 group by sex ;
-- 查询年龄大于 25 岁的人，按性别分组，统计每组的人数，并只显示性别人数大于 2 的数据 以下代码是否正确？SELECT sex, COUNT(*) FROM student3 WHERE age > 25 GROUP BY sex WHERE COUNT(*) >2;-- 正確寫法-- 对分组查询的结果再进行过滤SELECT sex, COUNT(*) FROM student3 WHERE age > 25 GROUP BY sex having COUNT(*) >2;
```

#### having與where 的區別

| 子名       | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| wher子句   | 1) 對查詢結果進行分組前,將不符合where條件的行去掉,即在分組之前過濾數據,即先過濾再分組. 2)where 後面不可以使用聚合函數 |
| having子句 | 1) having 子句後面的作用是篩選滿足條件的組,即在分組之後過濾數據,即先分組再過濾. 2) having後面可以使用聚合函數 |

#### limit語句

```
INSERT INTO student3(id,NAME,age,sex,address,math,english) VALUES(9,'唐僧',25,'男','长安',87,78),(10,'孙悟空',18,'男','花果山',100,66),(11,'猪八戒',22,'男','高老庄',58,78),(12,'沙僧',50,'男','流沙河',77,88),(13,'白骨精',22,'女','白虎岭',66,66),(14,'蜘蛛精',23,'女','盘丝洞',88,88);
```

limit 的作用：LIMIT 是限制的意思，所以 LIMIT 的作用就是限制查询记录的条数。

語法如下:

SELECT *|字段列表 [as 别名] FROM 表名 [WHERE 子句] [GROUP BY 子句] [HAVING 子句] [ORDER BY 子 句] [LIMIT 子句];

LIMIT offset,length;
offset：起始行数，从 0 开始计数，如果省略，默认就是 0
length： 返回的行数

```
-- 查询学生表中数据，从第 3 条开始显示，显示 6 条。select * from student3 limit 2,6;
```

分页：比如我们登录京东，淘宝，返回的商品信息可能有几万条，不是一次全部显示出来。是一页显示固定的
条数。 假设我们每页显示 5 条记录的方式来分页。

```
-- 如果第一个参数是 0 可以省略写：select * from student3 limit 5;-- 最后如果不够 5 条，有多少显示多少select * from student3 limit 10,5;
```

## 六 數據庫備份與還原

### 备份的应用场景

在服务器进行数据传输、数据存储和数据交换，就有可能产生数据故障。比如发生意外停机或存储介质损坏。
这时，如果没有采取数据备份和数据恢复手段与措施，就会导致数据的丢失，造成的损失是无法弥补与估量的。

### 备份与还原的语句

备份格式： DOS 下，未登录的时候。这是一个可执行文件 exe，在 bin 文件夹

```
mysqldump -u 用户名 -p 密码 数据库 > 文件的路径
```

还原格式：mysql 中的命令，需要登录后才可以操作

```
USE 数据库；SOURCE 导入文件的路径;
```

备份操作：

```
-- 备份 day21 数据库中的数据到 d:\day21.sql 文件中mysqldump -uroot -proot day21 > d:/day21.sql
```

导出结果：数据库中的所有表和数据都会导出成 SQL 语句

还原操作 还原 db1 数据库中的数据，注意：还原的时候需要先登录 MySQL,并选中对应的数据库。

1) 删除 day21 数据库中的所有表
2) 登录 MySQL
3) 选中数据库
4) 使用 SOURCE 命令还原数据
5) 查看还原结果

```
use day21;source d:/day21.sql;
```

### 图形化界面备份与还原

看老師操作

## 七 数据库表的约束

### 数据库约束的概述

约束的作用：

对表中的数据进行限制，保证数据的正确性、有效性和完整性。一个表如果添加了约束，不正确的数据将无
法插入到表中。约束在创建表的时候添加比较合适。

约束种类：

| 約束名   | 約束關鍵字             |
| -------- | ---------------------- |
| 主鍵     | primary key            |
| 唯一     | unique                 |
| 非空     | not nul                |
| 外键     | foreign key            |
| 检查约束 | check 注：mysql 不支持 |

#### 主键约束 : 用来唯一标识数据库中的每一条记录

通常不用业务字段作为主键，单独给每张表设计一个 id 的字段，把 id 作为主键。主键是给数据库和程序使用
的，不是给最终的客户使用的。所以主键有没有含义没有关系，只要不重复，非空就行。
如：身份证，学号不建议做成主键

创建主键:

- 主键关键字： primary key

- 主键的特点：

  1. 非空 not null
  2. 唯一

- 創建主鍵的方式

  1. 在創建表的時候給字段添加主鍵

     ```
     字段名 字段类型 PRIMARY KEY
     ```

1. 在已有表中添加主键

   \~~~mysql
   ALTER TABLE 表名 ADD PRIMARY KEY(字段名);

   — 创建表学生表 st5, 包含字段(id, name, age)将 id 做为主键
   create table st5 (
   id int primary key, — id 为主键
   name varchar(20),
   age int
   )
   desc st5;

— 插入重复的主键值
insert into st5 values (1, ‘关羽’, 30);
— 错误代码： 1062 Duplicate entry ‘1’ for key ‘PRIMARY’
insert into st5 values (1, ‘关云长’, 20);
select * from st5;
— 插入 NULL 的主键值, Column ‘id’ cannot be null
insert into st5 values (null, ‘关云长’, 20);

— 删除 st5 表的主键
alter table st5 drop primary key;
— 添加主键
alter table st5 add primary key(id);

```
  主键自增:  主键如果让我们自己添加很有可能重复,我们通常希望在每次插入新记录时,数据库自动生成主键字段的值  ~~~mysql  AUTO_INCREMENT 表示自动增长(字段类型必须是整数类型)  -- 插入数据  insert into st6 (name,age) values ('小乔',18);  insert into st6 (name,age) values ('大乔',20);  -- 另一种写法  insert into st6 values(null,'周瑜',35);  select * from st6;
```

修改自增长的默认值起始值:

默认地 AUTO_INCREMENT 的开始值是 1，如果希望修改起始值,请使用下列 SQL 语法

```
-- 指定起始值为 1000create table st4 (id int primary key auto_increment,name varchar(20)) auto_increment = 1000;insert into st4 values (null, '孔明');select * from st4;
```

創建好以後修改起始值

```
ALTER TABLE 表名 AUTO_INCREMENT=起始值;alter table st4 auto_increment = 2000;insert into st4 values (null, '刘备');
```

DELETE 和 TRUNCATE 对自增长的影响:

DELETE：删除所有的记录之后，自增长没有影响。

TRUNCATE：删除以后，自增长又重新开始。

#### 唯一約束 表中某一列不能出现重复的值

字段名 字段类型 UNIQUE

```
-- 创建学生表 st7, 包含字段(id, name),name 这一列设置唯一约束,不能出现同名的学生create table st7 (id int,name varchar(20) unique)-- 添加一个同名的学生insert into st7 values (1, '张三');select * from st7;-- Duplicate entry '张三' for key 'name'insert into st7 values (2, '张三');-- 重复插入多个 null 会怎样？insert into st7 values (2, null);insert into st7 values (3, null);
```

#### 非空約束 某一列不能为 null。

```
字段名 字段类型 NOT NULL-- 创建表学生表 st8, 包含字段(id,name,gender)其中 name 不能为 NULLcreate table st8 (id int,name varchar(20) not null,gender char(1))-- 添加一条记录其中姓名不赋值insert into st8 values (1,'张三疯','男');select * from st8;
```

#### 默認值 就是字段默認的值

```
字段名 字段类型 DEFAULT 默认值-- 创建一个学生表 st9，包含字段(id,name,address)， 地址默认值是广州create table st9 (id int,name varchar(20),address varchar(20) default '广州')-- 添加一条记录,使用默认地址insert into st9 values (1, '李四', default);select * from st9;insert into st9 (id,name) values (2, '李白');-- 添加一条记录,不使用默认地址insert into st9 values (3, '李四光', '深圳');
```

注意 如果一个字段设置了非空与唯一约束，该字段与主键的区别?

1. 主键数在一个表中，只能有一个。不能出现多个主键。主键可以单列，也可以是多列。
2. 自增长只能用在主键上

#### 外鍵約束

单表的缺点:

创建一个员工表包含如下列(id, name, age, dep_name, dep_location),id 主键并自动增长,添加 5 条数据

```
CREATE TABLE emp (id INT PRIMARY KEY AUTO_INCREMENT,NAME VARCHAR(30),age INT,dep_name VARCHAR(30),dep_location VARCHAR(30));-- 添加数据INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('张三', 20, '研发部', '广州');INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('李四', 21, '研发部', '广州');INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('王五', 20, '研发部', '广州');INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('老王', 20, '销售部', '深圳');INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('大王', 22, '销售部', '深圳');INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('小王', 18, '销售部', '深圳');
```

以上数据表的缺点:

1. 數據冗餘
2. 後期還會出現增刪改的問題

解決方案

```
-- 解决方案：分成 2 张表-- 创建部门表(id,dep_name,dep_location)-- 一方，主表create table department(id int primary key auto_increment,dep_name varchar(20),dep_location varchar(20));-- 创建员工表(id,name,age,dep_id)-- 多方，从表create table employee(id int primary key auto_increment,name varchar(20),age int,dep_id int -- 外键对应主表的主键)-- 添加 2 个部门insert into department values(null, '研发部','广州'),(null, '销售部', '深圳');select * from department;-- 添加员工,dep_id 表示员工所在的部门INSERT INTO employee (NAME, age, dep_id) VALUES ('张三', 20, 1);INSERT INTO employee (NAME, age, dep_id) VALUES ('李四', 21, 1);INSERT INTO employee (NAME, age, dep_id) VALUES ('王五', 20, 1);INSERT INTO employee (NAME, age, dep_id) VALUES ('老王', 20, 2);INSERT INTO employee (NAME, age, dep_id) VALUES ('大王', 22, 2);INSERT INTO employee (NAME, age, dep_id) VALUES ('小王', 18, 2);select * from employee;
```

**問題** : 当我们在 employee 的 dep_id 里面输入不存在的部门,数据依然可以添加.但是并没有对应的部门，
实际应用中不能出现这种情况。employee 的 dep_id 中的数据只能是 department 表中存在的 id

**目標**: 需要约束 dep_id 只能是 department 表中已经存在 id

**解決方式**: 使用外键约束

什么是外键约束?

- 什么是外键：在从表中与主表主键对应的那一列，如：员工表中的 dep_id
- 主表： 一方，用来约束别人的表
- 从表： 多方，被别人约束的表

创建约束的语法:

新建表时增加外键:

```
[CONSTRAINT] [外键约束名称] FOREIGN KEY(外键字段名) REFERENCES 主表名(主键字段名)
```

已有表增加外键:

```
ALTER TABLE 从表 ADD [CONSTRAINT] [外键约束名称] FOREIGN KEY (外键字段名) REFERENCES 主表(主键字段名);
```

具体操作：

```
-- 1) 删除副表/从表 employeedrop table employee;-- 2) 创建从表 employee 并添加外键约束 emp_depid_fk-- 多方，从表create table employee(id int primary key auto_increment,name varchar(20),age int,dep_id int, -- 外键对应主表的主键-- 创建外键约束constraint emp_depid_fk foreign key (dep_id) references department(id))-- 3) 正常添加数据INSERT INTO employee (NAME, age, dep_id) VALUES ('张三', 20, 1);INSERT INTO employee (NAME, age, dep_id) VALUES ('李四', 21, 1);INSERT INTO employee (NAME, age, dep_id) VALUES ('王五', 20, 1);INSERT INTO employee (NAME, age, dep_id) VALUES ('老王', 20, 2);INSERT INTO employee (NAME, age, dep_id) VALUES ('大王', 22, 2);INSERT INTO employee (NAME, age, dep_id) VALUES ('小王', 18, 2);select * from employee;-- 4) 部门错误的数据添加失败-- 插入不存在的部门-- Cannot add or update a child row: a foreign key constraint failsINSERT INTO employee (NAME, age, dep_id) VALUES ('老张', 18, 6);
```

删除外键

```
ALTER TABLE 从表 drop foreign key 外键名称;
-- 删除 employee 表的 emp_depid_fk 外键alter table employee drop foreign key emp_depid_fk;-- 在 employee 表情存在的情况下添加外键alter table employee add constraint emp_depid_fkforeign key (dep_id) references department(id);
```

外键的级联:

出現的新問題:

```
select * from employee;select * from department;-- 要把部门表中的 id 值 2，改成 5，能不能直接更新呢？-- Cannot delete or update a parent row: a foreign key constraint failsupdate department set id=5 where id=2;-- 要删除部门 id 等于 1 的部门, 能不能直接删除呢？-- Cannot delete or update a parent row: a foreign key constraint failsdelete from department where id=1;
```

什么是级联操作：

在修改和删除主表的主键时，同时更新或删除副表的外键值，称为级联操作

ON UPDATE CASCADE 级联更新，只能是创建表的时候创建级联关系。更新主表中的主键，从表中的外键
列也自动同步更新

ON DELETE CASCADE 级联删除

```
-- 删除 employee 表，重新创建 employee 表，添加级联更新和级联删除drop table employee;create table employee(id int primary key auto_increment,name varchar(20),age int,dep_id int, -- 外键对应主表的主键-- 创建外键约束constraint emp_depid_fk foreign key (dep_id) referencesdepartment(id) on update cascade on delete cascade)-- 再次添加数据到员工表和部门表INSERT INTO employee (NAME, age, dep_id) VALUES ('张三', 20, 1);INSERT INTO employee (NAME, age, dep_id) VALUES ('李四', 21, 1);INSERT INTO employee (NAME, age, dep_id) VALUES ('王五', 20, 1);INSERT INTO employee (NAME, age, dep_id) VALUES ('老王', 20, 2);INSERT INTO employee (NAME, age, dep_id) VALUES ('大王', 22, 2);INSERT INTO employee (NAME, age, dep_id) VALUES ('小王', 18, 2);-- 删除部门表？能不能直接删除？drop table department;-- 把部门表中 id 等于 1 的部门改成 id 等于 10update department set id=10 where id=1;select * from employee;select * from department;-- 删除部门号是 2 的部门delete from department where id=2;
```

数据约束小结:

| 約束名 | 關鍵字      | 說明                         |
| ------ | ----------- | ---------------------------- |
| 主键   | primary key | 1) 唯一  2) 非空             |
| 默认   | default     | 如果一列没有值，使用默认值   |
| 非空   | not null    | 这一列必须有值               |
| 唯一   | unique      | 这一列不能有重复值           |
| 外键   | foreign key | 主表中主键列，在从表中外键列 |

## 八 表与表之间的关系

现实生活中，实体与实体之间肯定是有关系的，比如：老公和老婆，部门和员工，老师和学生等。那么我们在设计表的时候，就应该体现出表与表之间的这种关系！

表和表之間的三種關係

一對多: 最常用的關係,部門和員工

多對多: 學生選課表 和學生表,一門課程可以有多個學生選擇,一個學生選擇多門課程

一對一: 相對使用比較少,員工表, 簡歷表, 公民表, 護照表

### 一對多

一对多（1:n） 例如：班级和学生，部门和员工，客户和订单，分类和商品
一对多建表原则: 在从表(多方)创建一个字段,字段作为外键指向主表(一方)的主键

### 多對多

多对多（m:n） 例如：老师和学生，学生和课程，用户和角色
多对多关系建表原则: 需要创建第三张表，中间表中至少两个字段，这两个字段分别作为外键指向各自一方的
主键。

### 一對一

一对一（1:1） 在实际的开发中应用不多.因为一对一可以创建成一张表。
两种建表原则：

| 一對一的建表原則 | 說明                                                       |
| ---------------- | ---------------------------------------------------------- |
| 外鍵唯一         | 主表的主播和從表的外鍵(唯一),形成主外鍵關係,外鍵唯一UNIQUE |
| 外鍵是主鍵       | 主表的主鍵和從表的主鍵,形成主外鍵關係                      |

## 九 數據庫設計

#### 數據規範化

什麼是範式:

好的數據庫設計對數據庫的存儲性能和後期的程序開發,都會產生重要的影響.建立科學的,規範的數據庫就需要滿足一些規則來優化數據庫的設計和存儲,這些規則就稱之為範式.

三大範式

目前關係數據庫由六種範式:第一範式(1NF),第二範式(2NF),第三範式(3NF),巴斯-柯德範式(BCNF),第四範式(4NF),和第五範式(5NF,又稱之為完美範式).满足最低要求的范式是第一范式（1NF）。在第一范式的基础上进一步满足更多规范要求的称为第二范式（2NF），其余范式以次类推。一般说来，数据库只需满足第三范式(3NF）就行了。

#### 第一範式

概念: 数据库表的每一列都是不可分割的原子数据项，不能是集合、数组等非原子数据项。即表中的某个列有多个值时，必须拆分为不同的列。简而言之，第一范式每一列不可再拆分，称为原子性。

#### 第二範式

概念: 在满足第一范式的前提下，表中的每一个字段都完全依赖于主键。
所谓完全依赖是指不能存在仅依赖主键一部分的列。简而言之，第二范式就是在第一范式的基础上所有列完全依赖于主键列。当存在一个复合主键包含多个主键列的时候，才会发生不符合第二范式的情况。比如有一个主键有两个列，不能存在这样的属性，它只依赖于其中一个列，这就是不符合第二范式。

第二範式的特點:

1. 一張表只描述一件事情.
2. 表中的每一列都完全依賴于主鍵

一個例子

我們有一張借書證表,字段如下:

**學生證號**,學生證名稱,學生證辦理時間,**借書證號**,借書證名稱,借書辦理時間

這裡的學生證號和借書證號 不關聯

所以 我們應該拆分為兩張表

如下:

| 學生證號 | 學生證名稱 | 學生證辦理時間 |
| -------- | ---------- | -------------- |
| 借書證號 | 借書證名稱 | 借書證辦理時間 |

#### 第三範式

概念: 在满足第二范式的前提下，表中的每一列都直接依赖于主键，而不是通过其它的列来间接依赖于主键。
简而言之，第三范式就是所有列不依赖于其它非主键列，也就是在满足 2NF 的基础上，任何非主列不得传递
依赖于主键。所谓传递依赖，指的是如果存在”A → B → C”的决定关系，则 C 传递依赖于 A。因此，满足第三范
式的数据库表应该不存在如下依赖关系：主键列 → 非主键列 x → 非主键列 y;

實例;

學生信息表:

| 學號 | 姓名 | 年齡 | 所在學院 | 學院地點 |
| ---- | ---- | ---- | -------- | -------- |
|      |      |      |          |          |

存在傳遞的決定關係:

學號—>所在學院—> 學院地點

所以需要拆分成兩張表

| 學號 | 姓名 | 年齡 | 所在學院的編號(外鍵) |
| ---- | ---- | ---- | -------------------- |
|      |      |      |                      |

| 學院編號 | 所在學院 | 學院地點 |
| -------- | -------- | -------- |
|          |          |          |

#### 三大範式小結:

| 範式 | 特點                                                         |
| ---- | ------------------------------------------------------------ |
| 1NF  | 原子性：表中每列不可再拆分。                                 |
| 2NF  | 不产生局部依赖，一张表只描述一件事情                         |
| 3NF  | 不产生传递依赖，表中每一列都直接依赖于主键。而不是通过其它列间接依赖于主键。 |

## 十 多表连接

### 什么是多表查询?

数据准备

```
# 创建部门表create table dept(id int primary key auto_increment,name varchar(20))insert into dept (name) values ('开发部'),('市场部'),('财务部');# 创建员工表create table emp (id int primary key auto_increment,name varchar(10),gender char(1), -- 性别salary double, -- 工资join_date date, -- 入职日期dept_id int,foreign key (dept_id) references dept(id) -- 外键，关联部门表(部门表的主键))insert into emp(name,gender,salary,join_date,dept_id) values('孙悟空','男',7200,'2013-02-24',1);insert into emp(name,gender,salary,join_date,dept_id) values('猪八戒','男',3600,'2010-12-02',2);insert into emp(name,gender,salary,join_date,dept_id) values('唐僧','男',9000,'2008-08-08',2);insert into emp(name,gender,salary,join_date,dept_id) values('白骨精','女',5000,'2015-10-07',3);insert into emp(name,gender,salary,join_date,dept_id) values('蜘蛛精','女',4500,'2011-03-14',1);
```

多表查询的作用:

比如：我们想查询孙悟空的名字和他所在的部门的名字，则需要使用多表查询。如果一条 SQL 语句查询多张表，因为查询结果在多张不同的表中。每张表取 1 列或多列。

多表查询的分类

- 多表查询

  ```
    1.  内连接       - 隐式内连接      - 显示内连接  2.  外连接          - 左外连接          - 右外连接
  ```



### 笛卡儿积现象

什么是笛卡尔积现象

```
-- 需求：查询所有的员工和所有的部门select * from emp,dept;
```

![1](file:///C:/Users/71482/Desktop/%E6%96%B0%E5%BB%BA%E6%96%87%E4%BB%B6%E5%A4%B9/imgs/1.png)



怎么消除笛卡尔积现象?

我们发现不是所有的数据组合都是有用的，只有员工表.dept_id = 部门表.id 的数据才是有用的。所以需要通过条件过滤掉没用的数据。

```
-- 设置过滤条件 Column 'id' in where clause is ambiguousselect * from emp,dept where id=5;select * from emp,dept where emp.`dept_id` = dept.`id`;-- 查询员工和部门的名字select emp.`name`, dept.`name` from emp,dept where emp.`dept_id` = dept.`id`;
```

### 内连接

用**左边表**的记录去匹配**右边表**的记录，如果符合条件的则显示。如：从表.外键=主表.主键

#### 隐式内连接

- 隐式内连接：看不到 JOIN 关键字，条件使用 WHERE 指定

  语法:

  SELECT 字段名 FROM 左表, 右表 WHERE 条件

  ```
  select * from emp,dept where emp.`dept_id` = dept.`id`;
  ```

#### 显示内连接

显示内连接：使用 INNER JOIN … ON 语句, 可以省略 INNER

语法:

SELECT 字段名 FROM 左表 [INNER] JOIN 右表 ON 条件

查询唐僧的信息，显示员工 id，姓名，性别，工资和所在的部门名称，我们发现需要联合 2 张表同时才能查询出需要的数据，使用内连接

1) 确定查询那些表

```
select * from emp inner join dept;
```

2) 确定表连接条件，员工表.dept_id = 部门表.id 的数据才是有效的

```
select * from emp e inner join dept d on e.`dept_id` = d.`id`;
```

3) 确定查询条件，我们查询的是唐僧的信息，员工表.name=’唐僧’

```
select * from emp e inner join dept d on e.`dept_id` = d.`id` where e.`name`='唐僧';
```

4) 确定查询字段，查询唐僧的信息，显示员工 id，姓名，性别，工资和所在的部门名称

```
select e.`id`,e.`name`,e.`gender`,e.`salary`,d.`name` from emp e inner join dept don e.`dept_id` = d.`id` where e.`name`='唐僧';
```

5) 我们发现写表名有点长，可以给表取别名，显示的字段名也使用别名

```
select e.`id` 编号,e.`name` 姓名,e.`gender` 性别,e.`salary` 工资,d.`name` 部门名字 fromemp e inner join dept d on e.`dept_id` = d.`id` where e.`name`='唐僧';
```

总结内连接查询步骤：

1) 确定查询哪些表
2) 确定表连接的条件
3) 确定查询的条件
4) 确定查询的字段

#### 左外连接

左外连接：使用 LEFT OUTER JOIN … ON，OUTER 可以省略

SELECT 字段名 FROM 左表 LEFT [OUTER] JOIN 右表 ON 条件

语法:

用左边表的记录去匹配右边表的记录，如果符合条件的则显示；否则，显示 NULL
可以理解为：在内连接的基础上保证左表的数据全部显示(左表是部门，右表员工)

```
-- 在部门表中增加一个销售部insert into dept (name) values ('销售部');select * from dept;-- 使用内连接查询select * from dept d inner join emp e on d.`id` = e.`dept_id`;-- 使用左外连接查询select * from dept d left join emp e on d.`id` = e.`dept_id`;
```

#### 外右连接

右外连接：使用 RIGHT OUTER JOIN … ON，OUTER 可以省略

语法:

SELECT 字段名 FROM 左表 RIGHT [OUTER ]JOIN 右表 ON 条件

用右边表的记录去匹配左边表的记录，如果符合条件的则显示；否则，显示 NULL可以理解为：在内连接的基础上保证右表的数据全部显示

```
-- 在员工表中增加一个员工insert into emp values (null, '沙僧','男',6666,'2013-12-05',null);select * from emp;-- 使用内连接查询select * from dept inner join emp on dept.`id` = emp.`dept_id`;-- 使用右外连接查询select * from dept right join emp on dept.`id` = emp.`dept_id`;
```

### 子查询

#### 什么是子查询

```
-- 需求：查询开发部中有哪些员工select * from emp;-- 通过两条语句查询select id from dept where name='开发部' ;select * from emp where dept_id = 1;-- 使用子查询select * from emp where dept_id = (select id from dept where name='市场部');
```

子查询的概念：

1) 一个查询的结果做为另一个查询的条件
2) 有查询的嵌套，内部的查询称为子查询
3) 子查询要使用括号

#### 子查询结果的三种情况

1) 子查询的结果是单行单列

2) 子查询的结果是多行单列

3) 子查询的结果是多行多列

#### 子查询的结果是一个值的时候

子查询结果只要是单行单列，肯定在 WHERE 后面作为条件，父查询使用：比较运算符，如：> 、<、<>、=等;

语法:

SELECT 查询字段 FROM 表 WHERE 字段=（子查询）;

查询工资最高的员工是谁?

```
-- 1) 查询最高工资是多少select max(salary) from emp;-- 2) 根据最高工资到员工表查询到对应的员工信息select * from emp where salary = (select max(salary) from emp);
```

查询开发部与财务部所有的员工信息

```
-- 先查询开发部与财务部的 idselect id from dept where name in('开发部','财务部');-- 再查询在这些部门 id 中有哪些员工select * from emp where dept_id in (select id from dept where name in('开发部','财务部'));
```

子查询的结果是多行多列

子查询结果只要是多列，肯定在 FROM 后面作为表

语法:

SELECT 查询字段 FROM （子查询） 表别名 WHERE 条件;

子查询作为表需要取别名，否则这张表没有名称则无法访问表中的字段

查询出 2011 年以后入职的员工信息，包括部门名称

```
-- 查询出 2011 年以后入职的员工信息，包括部门名称-- 在员工表中查询 2011-1-1 以后入职的员工select * from emp where join_date >='2011-1-1';-- 查询所有的部门信息，与上面的虚拟表中的信息组合，找出所有部门 id 等于的 dept_idselect * from dept d, (select * from emp where join_date >='2011-1-1') e whered.`id`= e.dept_id ;-- 也可以使用表连接:select * from emp inner join dept on emp.`dept_id` = dept.`id` wherejoin_date >='2011-1-1';select * from emp inner join dept on emp.`dept_id` = dept.`id` andjoin_date >='2011-1-1';
```

子查询小结

子查询结果只要是单列，则在 WHERE 后面作为条件

子查询结果只要是多列，则在 FROM 后面作为表进行二次查询

## 十一 事务

### 11.1 事务的应用场景说明

什么是事务： 在实际的开发过程中，一个业务操作如：转账，往往是要多次访问数据库才能完成的。转
账是一个用户扣钱，另一个用户加钱。如果其中有一条 SQL 语句出现异常，这条 SQL 就可能执行失败。

事务执行是一个整体，所有的 SQL 语句都必须执行成功。如果其中有 1 条 SQL 语句出现异常，则所有的SQL 语句都要回滚，整个业务执行失败。

转账的操作

```
-- 创建数据表CREATE TABLE account (id INT PRIMARY KEY AUTO_INCREMENT,NAME VARCHAR(10),balance DOUBLE);-- 添加数据INSERT INTO account (NAME, balance) VALUES ('张三', 1000), ('李四', 1000);
```

模拟张三给李四转 500 元钱，一个转账的业务操作最少要执行下面的 2 条语句：
张三账号-500
李四账号+500

```
-- 张三账号-500update account set balance = balance - 500 where name='张三';-- 李四账号+500update account set balance = balance + 500 where name='李四';
```

假设当张三账号上-500 元,服务器崩溃了。李四的账号并没有+500 元，数据就出现问题了。我们需要保证其中一条 SQL 语句出现问题，整个转账就算失败。只有两条 SQL 都成功了转账才算成功。这个时候就需要用到事务。

### 11.2手动提交事务

MYSQL 中可以有两种方式进行事务的操作：
1) 手动提交事务
2) 自动提交事务

手动提交事务的 SQL 语句

| 功能     | sql语句           |
| -------- | ----------------- |
| 开启事务 | start transaction |
| 提交事务 | commit;           |
| 回滚事务 | rollback;         |

手动提交事务使用过程：

1)执行成功的情况： 开启事务  执行多条 SQL 语句  成功提交事务

2) 执行失败的情况： 开启事务  执行多条 SQL 语句  事务的回滚

案例演示 1：事务提交

模拟张三给李四转 500 元钱（成功） 目前数据库数据如下：

1)使用 DOS 控制台进入 MySQL
2)执行以下 SQL 语句： 1.开启事务， 2.张三账号-500， 3.李四账号+500
3)使用 SQLYog 查看数据库：发现数据并没有改变
4)在控制台执行 commit 提交事务：
5)使用 SQLYog 查看数据库：发现数据改变

案例演示 2：事务回滚

模拟张三给李四转 500 元钱（失败） 目前数据库数据如下：

1) 在控制台执行以下 SQL 语句：1.开启事务， 2.张三账号-500
2) 使用 SQLYog 查看数据库：发现数据并没有改变
3) 在控制台执行 rollback 回滚事务：
4) 使用 SQLYog 查看数据库：发现数据没有改变

总结: 如果事务中 SQL 语句没有问题，commit 提交事务，会对数据库数据的数据进行改变。 如果事务中 SQL
语句有问题，rollback 回滚事务，会回退到开启事务时的状态。

### 11.3 自动提交事务

MySQL 默认每一条 DML(增删改)语句都是一个单独的事务，每条语句都会自动开启一个事务，语句执行完毕
自动提交事务，MySQL 默认开始自动提交事务

案例演示 3：自动提交事务

1. 将金额重置为 1000
   2.更新其中某一个账户
   3.使用 SQLYog 查看数据库：发现数据已经改变

查看 MySQL 是否开启自动提交事务

@@表示全局变量，1 表示开启，0 表示关闭

![2](file:///C:/Users/71482/Desktop/%E6%96%B0%E5%BB%BA%E6%96%87%E4%BB%B6%E5%A4%B9/imgs/2.png)

取消自动提交事务

![3](file:///C:/Users/71482/Desktop/%E6%96%B0%E5%BB%BA%E6%96%87%E4%BB%B6%E5%A4%B9/imgs/3.png)

执行更新语句，使用 SQLYog 查看数据库，发现数据并没有改变

在控制台执行 commit 提交任务

![4](file:///C:/Users/71482/Desktop/%E6%96%B0%E5%BB%BA%E6%96%87%E4%BB%B6%E5%A4%B9/imgs/4.png)

### 11.4 事务原理

事务开启之后, 所有的操作都会临时保存到事务日志中, 事务日志只有在得到 commit 命令才会同步到数据表中，其他任何情况都会清空事务日志(rollback，断开连接)

原理图:

![5](https://www.kuangstudy.com/imgs/5.png)

事务的步骤：

1)客户端连接数据库服务器，创建连接时创建此用户临时日志文件
2)开启事务以后，所有的操作都会先写入到临时日志文件中
3)所有的查询操作从表中查询，但会经过日志文件加工后才返回
4)如果事务提交则将日志文件中的数据写到表中，否则清空日志文件。

### 11.5 回滚点

什么是回滚点:在某些成功的操作完成之后，后续的操作有可能成功有可能失败，但是不管成功还是失败，前面操作都已经成
功，可以在当前成功的位置设置一个回滚点。可以供后续失败操作返回到该位置，而不是返回所有操作，这个点称
之为回滚点。

回滚点的操作语句:

| 回滚点的操作语句 | 语句             |
| ---------------- | ---------------- |
| 设置回滚点       | savepoint 名字   |
| 回到回滚点       | rollback to 名字 |
|                  |                  |

具体操作：

1) 将数据还原到 1000
2) 开启事务
3) 让张三账号减 3 次钱，每次 10 块
4) 设置回滚点：savepoint three_times;
5) 让张三账号减 4 次钱，每次 10 块
6) 回到回滚点：rollback to three_times;
7) 分析执行过程

总结：设置回滚点可以让我们在失败的时候回到回滚点，而不是回到事务开启的时候。

### 11.6 事务的隔离级别

#### 11.6.1 事务的四大特性

| 事务特性            | 含义                                                         |
| ------------------- | ------------------------------------------------------------ |
| 原子性(Atomicity)   | 每个事物都是一个整体,不可再拆分,事物中所有的SQL语句要么都执行成功,要么都失败 |
| 一致性(Consistency) | 事物在执行前数据库的状态与执行后数据库的状态保持一致,如:转账前2个人的总金额是2000,转账后2人总金额也是2000 |
| 隔离性(lsolation)   | 事务与事务之间不应该相互影响,执行时保持隔离的状态.           |
| 持久性(Durability)  | 一旦事务执行成功,对数据库的修改是持久的.就算关机,也是保存下来的. |

#### 11.6.2 事务的隔离级别

事物在操作是的理想状态: 所有事物之间保持隔离,相互不影响.因为并发操作,多个用户同时访问同一个数据.可能引发并发访问的问题:

| 并发访问的问题 | 含义                                                         |
| -------------- | ------------------------------------------------------------ |
| 脏读           | 一个事务读取到了另一个事务中尚未提交的数据                   |
| 不可重复读     | 一个事务中两次读取的数据内容不一致,要求的是一个事务中多次读取的数据是一致的,这是事务update时引发的问题 |
| 幻读           | 一个事务中两次读取的数据的数量不一致,要求在一个事务多次读取的数据的数量是一致的,这是insert或delete 时引发的问题 |

#### 11.6.3 mysql 数据库有四种隔离级别

隔离级别1位低, 4为最高; 隔离级别越高,性能越差 安全性越高;

| 级别 | 名字     | 隔离级别         | 脏读 | 不可重复读 | 幻读 | 数据库默认隔离级别  |
| ---- | -------- | :--------------- | ---- | ---------- | ---- | ------------------- |
| 1    | 读未提交 | read uncommitted | 是   | 是         | 是   |                     |
| 2    | 读已提交 | read committed   | 否   | 是         | 是   | Oracle 和sql server |
| 3    | 可重复读 | repeatable read  | 否   | 否         | 是   | mysql               |
| 4    | 串行化   | serializable     | 否   | 否         | 否   |                     |

#### 11.6.4 mysql事务隔离级别相关命令

```
-- 查看全局事务的隔离级别select @@tx_isolation;-- 设置事务隔离级别,需要退出msyql 在重新登录才能看见隔离级别的变化set global transaction isolation level "级别的字符串";
```

## 十二 存储过程

### 12.1 说明

MySQL中提供存储过程与存储函数机制，我们先将其统称为**存储程序**，一般的SQL语句需要先编译然后执行，存储程序是一组为了完成特定功能的SQL语句集，经编译后存储在数据库中，当用户通过指定存储程序的名字并给定参数（如果该存储程序带有参数）来调用才会执行;

### 12.2 存储程序的优缺点

#### 12.2.1 优点

通常存储过程有助于提高应用程序的性能。当创建，存储过程被编译之后，就存储在数据库中。 但是，MySQL实现的存储过程略有不同。 MySQL存储过程按需编译。 在编译存储过程之后，MySQL将其放入缓存中。 MySQL为每个连接维护自己的存储过程高速缓存。 如果应用程序在单个连接中多次使用存储过程，则使用编译版本，否则存储过程的工作方式类似于查询。

- 性能 : 存储过程有助于减少应用程序和数据库服务器之间的流量，因为应用程序不必发送多个冗长的SQL语句，而只能发送存储过程的名称和参数。
- 复用: 存储的程序对任何应用程序都是可重用的和透明的。 存储过程将数据库接口暴露给所有应用程序，以便开发人员不必开发存储过程中已支持的功能。
- 安全 : 存储的程序是安全的。 数据库管理员可以向访问数据库中存储过程的应用程序授予适当的权限，而不向基础数据库表提供任何权限。

#### 12.2.2 缺点

- 如果使用大量存储过程，那么使用这些存储过程的每个连接的内存使用量将会大大增加。 此外，如果在存储过程中过度使用大量逻辑操作，则CPU使用率也会增加，因为数据库服务器的设计不偏于逻辑运算。
- 很难调试存储过程。只有少数数据库管理系统允许调试存储过程。不幸的是，MySQL不提供调试存储过程的功能。

### 12.3 数据准备

```
-- 创建数据库 注意编码create database mytest character set utf8;use mytest;-- 创建测试表 classDROP TABLE IF EXISTS `class`;CREATE TABLE `class` (  `id` int(11) NOT NULL AUTO_INCREMENT,  `name` varchar(30) DEFAULT NULL,  PRIMARY KEY (`id`)) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;-- 创建表 studentDROP TABLE IF EXISTS `student`;CREATE TABLE `student` (  `id` int(11) NOT NULL AUTO_INCREMENT,  `name` varchar(20) DEFAULT NULL,  `class_id` int(11) DEFAULT NULL,  PRIMARY KEY (`id`)) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;-- 插入数据 insert  into `class`(`id`,`name`) values (1,'Java'),(2,'UI'),(3,'产品');insert  into `student`(`id`,`name`,`class_id`) values (1,'张三',1),(2,'李四',1),(3,'王五',2),(4,'赵刘',1),(5,'钱七',3);
```

### 12.4 存储过程的使用

#### 12.4.1 语法

```
CREATE PROCEDURE procedure_name ([parameters[,...]])begin-- SQL语句end ;
```

#### 12.4.2 示例

```
-- 创建存储过程create procedure test1()begin    select 'Hello';end;-- 调用存储过程call test1();
-- 查看mytest 数据库中的所有存储过程select name from mysql.proc where db='mytest';-- 查看存储过程的状态信息show procedure status;-- 查看存储过程的创建语句show create procedure test1;-- 删除 存储过程drop procedure test1;
```

### 12.5 存储过程的高级语法

#### 12.5.1 变量

- declare : 声明变量

```
CREATE PROCEDURE test2 ()begin    declare num int default 0;        -- 声明变量,赋默认值为0    select num+10;end ;call test2();            -- 调用存储过程
```

- set : 赋值操作

  ```
  CREATE PROCEDURE test3 ()begin    declare num int default 0;    set num =20;            -- 给num变量赋值    select num;end ;call test3();
  ```

- into: 赋值

  ```
  CREATE PROCEDURE test4 ()begin    declare num int default 0;                select count(1) into num from student;    select num;end ;call test4();
  ```

  #### 12.5.2 if语句

  - 根据class_id 判断是java还是ui还是产品

  ```
  CREATE PROCEDURE test5 ()begin    declare id int default 1;                declare class_name varchar(30);    if id=1 then        set class_name='哇塞，Java大佬！';    elseif id=2 then        set class_name='原来是UI的啊';    else        set class_name='不用想了，肯定是产品小样';    end if;    select class_name;end ;call test5();
  ```

  #### 12.5.3 传递参数

  语法:

  ```
  create procedure procedure_name([in/out/input] 参数名 参数类型)
  ```

  - **in**：该参数可以作为输入，也就是需要调用方传入值 , 默认
  - **out**：该参数作为输出，也就是该参数可以作为返回值
  - **inout**：既可以作为输入参数，也可以作为输出参数

  ##### 12.5.3.1 in-输入参数

  ```
  -- 定义一个输入参数CREATE PROCEDURE test6 (in id int)begin    declare class_name varchar(30);    if id=1 then        set class_name='哇塞，Java大佬！';    elseif id=2 then        set class_name='原来是UI的啊';    else        set class_name='不用想了，肯定是产品小样';    end if;    select class_name;end ;call test6(3);
  ```

  ##### 12.5.3.2 out-输出参数

  \~~~mysql
  — 定义一个输入参数和一个输出参数
  CREATE PROCEDURE test7 (in id int,out class_name varchar(100))
  begin

  ```
  if id=1 then    set class_name='哇塞，Java大佬！';elseif id=2 then    set class_name='原来是UI的啊';else    set class_name='不用想了，肯定是产品小样';end if;
  ```

  end ;

call test7(1,[@class_name](https://github.com/class_name)); — 创建会话变量

select [@class_name](https://github.com/class_name); — 引用会话变量

```
  @xxx：代表定义一个会话变量，整个会话都可以使用，当会话关闭（连接断开）时销毁  @@xxx：代表定义一个系统变量，永久生效。#### 12.5.4 case 语句- 需求 传递一个月份值,返回所在的季节.~~~mysqlCREATE PROCEDURE test8 (in month int,out season varchar(10))begin    case         when month >=1 and month<=3 then            set season='spring';        when month >=4 and month<=6 then            set season='summer';        when month >=7 and month<=9 then            set season='autumn';        when month >=10 and month<=12 then            set season='winter';    end case;end ;call test8(9,@season);            -- 定义会话变量来接收test8存储过程返回的值select @season;
```

#### 12.5.5 while循环

- 需求 计算任意数的累加和

```
CREATE PROCEDURE test10 (in count int)begin    declare total int default 0;    declare i int default 1;    while i<=count do        set total=total+i;        set i=i+1;    end while;    select total;end ;call test10(10);
```

#### 12.5.6 repeat 循环

- 需求 计算任意数的累加和

```
CREATE PROCEDURE test11 (count int)        -- 默认是输入(in)参数begin    declare total int default 0;    repeat         set total=total+count;        set count=count-1;        until count=0                -- 结束条件,注意不要打分号    end repeat;    select total;end ;call test11(10);
```

#### 12.5.7 loop循环

- 需求 计算任意数的累加和

  ```
  CREATE PROCEDURE test12 (count int)        -- 默认是输入(in)参数begin    declare total int default 0;        sum:loop                            -- 定义循环标识        set total=total+count;        set count=count-1;        if count < 1 then            leave sum;                    -- 跳出循环        end if;    end loop sum;                        -- 标识循环结束    select total;end ;call test12(10);
  ```

  #### 12.5.8 游标

  游标是用来存储查询结果集的数据类型，可以帮我们保存多条行记录结果，我们要做的操作就是读取游标中的数据获取每一行的数据。

  - 声明游标

  ```
  declare cursor_name cursor for statement;
  ```

  - 打开游标

  ```
  open cursor_name;
  ```

  - 关闭游标

  ```
  close cursor_name;
  ```

  - 案例：

  ```
  CREATE PROCEDURE test13 ()        -- 默认是输入(in)参数begin    declare id int(11);    declare `name` varchar(20);    declare class_id int(11);    -- 定义游标结束标识符    declare has_data int default 1;    declare stu_result cursor for select * from student;    -- 监测游标结束    declare exit handler for not FOUND set has_data=0;    -- 打开游标    open stu_result;    repeat         fetch stu_result into id,`name`,class_id;        select concat('id: ',id,';name: ',`name`,';class_id',class_id);        until has_data=0        -- 退出条件,注意不要打分号    end repeat;    -- 关闭游标    close stu_result;end ;call test13();
  ```

## 十三 触发器

### 13.1 什么是触发器

MySQL 的触发器和存储过程一样，都是嵌入到 MySQL 中的一段程序，是 MySQL 中管理数据的有力工具。不同的是执行存储过程要使用 CALL 语句来调用，而触发器的执行不需要使用 CALL 语句来调用，也不需要手工启动，而是通过对数据表的相关操作来触发、激活从而实现执行。比如当对 student 表进行操作（INSERT，DELETE 或 UPDATE）时就会激活它执行。

触发器与数据表关系密切，主要用于保护表中的数据。特别是当有多个表具有一定的相互联系的时候，触发器能够让不同的表保持数据的一致性。

在 MySQL 中，只有执行 INSERT、UPDATE 和 DELETE 操作时才能激活触发器，其它 SQL 语句则不会激活触发器。

那么为什么要使用触发器呢？比如，在实际开发项目时，我们经常会遇到以下情况：

- 在学生表中添加一条关于学生的记录时，学生的总数就必须同时改变。
- 增加一条学生记录时，需要检查年龄是否符合范围要求。
- 删除一条学生信息时，需要删除其成绩表上的对应记录。
- 删除一条数据时，需要在数据库存档表中保留一个备份副本。

虽然上述情况实现的业务逻辑不同，但是它们都需要在数据表发生更改时，自动进行一些处理。这时就可以使用触发器处理。例如，对于第一种情况，可以创建一个触发器对象，每当添加一条学生记录时，就执行一次计算学生总数的操作，这样就可以保证每次添加一条学生记录后，学生总数和学生记录数是一致的。

### 13.2 触发器的优缺点

#### 13.2.1 优点

- 触发器的执行是自动的，当对触发器相关表的数据做出相应的修改后立即执行。
- 触发器可以实施比 FOREIGN KEY 约束、CHECK 约束更为复杂的检查和操作。
- 触发器可以实现表数据的级联更改，在一定程度上保证了数据的完整性。

#### 13.2.2 缺点

- 使用触发器实现的业务逻辑在出现问题时很难进行定位，特别是涉及到多个触发器的情况下，会使后期维护变得困难。
- 大量使用触发器容易导致代码结构被打乱，增加了程序的复杂性，
- 如果需要变动的数据量较大时，触发器的执行效率会非常低。

### 13.3 MySQL 支持的触发器

在实际使用中，MySQL 所支持的触发器有三种：INSERT 触发器、UPDATE 触发器和 DELETE 触发器。

#### 13.3.1 INSERT 触发器

在 INSERT 语句执行之前或之后响应的触发器。

使用 INSERT 触发器需要注意以下几点：

- 在 INSERT 触发器代码内，可引用一个名为 NEW（不区分大小写）的虚拟表来访问被插入的行。
- 在 BEFORE INSERT 触发器中，NEW 中的值也可以被更新，即允许更改被插入的值（只要具有对应的操作权限）。
- 对于 AUTO_INCREMENT 列，NEW 在 INSERT 执行之前包含的值是 0，在 INSERT 执行之后将包含新的自动生成值。

#### 13.3.2 UPDATE 触发器

在 UPDATE 语句执行之前或之后响应的触发器。

使用 UPDATE 触发器需要注意以下几点：

- 在 UPDATE 触发器代码内，可引用一个名为 NEW（不区分大小写）的虚拟表来访问更新的值。
- 在 UPDATE 触发器代码内，可引用一个名为 OLD（不区分大小写）的虚拟表来访问 UPDATE 语句执行前的值。
- 在 BEFORE UPDATE 触发器中，NEW 中的值可能也被更新，即允许更改将要用于 UPDATE 语句中的值（只要具有对应的操作权限）。
- OLD 中的值全部是只读的，不能被更新。

注意：当触发器设计对触发表自身的更新操作时，只能使用 BEFORE 类型的触发器，AFTER 类型的触发器将不被允许。

#### 13.3.3 DELETE 触发器

在 DELETE 语句执行之前或之后响应的触发器。

使用 DELETE 触发器需要注意以下几点：

- 在 DELETE 触发器代码内，可以引用一个名为 OLD（不区分大小写）的虚拟表来访问被删除的行。
- OLD 中的值全部是只读的，不能被更新。

总体来说，触发器使用的过程中，MySQL 会按照以下方式来处理错误。

对于事务性表，如果触发程序失败，以及由此导致的整个语句失败，那么该语句所执行的所有更改将回滚；对于非事务性表，则不能执行此类回滚，即使语句失败，失败之前所做的任何更改依然有效。

若 BEFORE 触发程序失败，则 MySQL 将不执行相应行上的操作。

若在 BEFORE 或 AFTER 触发程序的执行过程中出现错误，则将导致调用触发程序的整个语句失败。

仅当 BEFORE 触发程序和行操作均已被成功执行，MySQL 才会执行 AFTER 触发程序。

### 13.4 基本语法

触发器是与 MySQL 数据表有关的数据库对象，在满足定义条件时触发，并执行触发器中定义的语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性;

在 MySQL 5.7 中，可以使用 CREATE TRIGGER 语句创建触发器。

语法格式如下：

```
CREATE <触发器名> < BEFORE | AFTER ><INSERT | UPDATE | DELETE >ON <表名> FOR EACH Row<触发器主体>
```

语法说明如下。

#### 13.4.1 触发器名

触发器的名称，触发器在当前数据库中必须具有唯一的名称。如果要在某个特定数据库中创建，名称前面应该加上数据库的名称。

#### 13.4.2 INSERT | UPDATE | DELETE

触发事件，用于指定激活触发器的语句的种类。

注意：三种触发器的执行时间如下。

- INSERT：将新行插入表时激活触发器。例如，INSERT 的 BEFORE 触发器不仅能被 MySQL 的 INSERT 语句激活，也能被 LOAD DATA 语句激活。
- DELETE： 从表中删除某一行数据时激活触发器，例如 DELETE 和 REPLACE 语句。
- UPDATE：更改表中某一行数据时激活触发器，例如 UPDATE 语句。

#### 13.4.3 BEFORE | AFTER

BEFORE 和 AFTER，触发器被触发的时刻，表示触发器是在激活它的语句之前或之后触发。若希望验证新数据是否满足条件，则使用 BEFORE 选项；若希望在激活触发器的语句执行之后完成几个或更多的改变，则通常使用 AFTER 选项。

#### 13.4.4 表名

与触发器相关联的表名，此表必须是永久性表，不能将触发器与临时表或视图关联起来。在该表上触发事件发生时才会激活触发器。同一个表不能拥有两个具有相同触发时刻和事件的触发器。例如，对于一张数据表，不能同时有两个 BEFORE UPDATE 触发器，但可以有一个 BEFORE UPDATE 触发器和一个 BEFORE INSERT 触发器，或一个 BEFORE UPDATE 触发器和一个 AFTER UPDATE 触发器。

#### 13.4.5 触发器主体

触发器动作主体，包含触发器激活时将要执行的 MySQL 语句。如果要执行多个语句，可使用 BEGIN…END 复合语句结构。

#### 13.4.6 FOR EACH ROW

一般是指行级触发，对于受触发事件影响的每一行都要激活触发器的动作。例如，使用 INSERT 语句向某个表中插入多行数据时，触发器会对每一行数据的插入都执行相应的触发器动作。

> 注意：每个表都支持 INSERT、UPDATE 和 DELETE 的 BEFORE 与 AFTER，因此每个表最多支持 6 个触发器。每个表的每个事件每次只允许有一个触发器。单一触发器不能与多个事件或多个表关联。

另外，在 MySQL 中，若需要查看数据库中已有的触发器，则可以使用 SHOW TRIGGERS 语句。

### 13.5 创建 BEFORE 类型触发器

在 test_db 数据库中，数据表 tb_emp8 为员工信息表，包含 id、name、deptId 和 salary 字段，数据表 tb_emp8 的表结构如下所示。

```
mysql> SELECT * FROM tb_emp8;Empty set (0.07 sec)mysql> DESC tb_emp8;+--------+-------------+------+-----+---------+-------+| Field  | Type        | Null | Key | Default | Extra |+--------+-------------+------+-----+---------+-------+| id     | int(11)     | NO   | PRI | NULL    |       || name   | varchar(22) | YES  | UNI | NULL    |       || deptId | int(11)     | NO   | MUL | NULL    |       || salary | float       | YES  |     | 0       |       |+--------+-------------+------+-----+---------+-------+4 rows in set (0.05 sec)
```

【实例 1】创建一个名为 SumOfSalary 的触发器，触发的条件是向数据表 tb_emp8 中插入数据之前，对新插入的 salary 字段值进行求和计算。输入的 SQL 语句和执行过程如下所示。

```
mysql> CREATE TRIGGER SumOfSalary    -> BEFORE INSERT ON tb_emp8    -> FOR EACH ROW    -> SET @sum=@sum+NEW.salary;Query OK, 0 rows affected (0.35 sec)
```

触发器 SumOfSalary 创建完成之后，向表 tb_emp8 中插入记录时，定义的 sum 值由 0 变成了 1500，即插入值 1000 和 500 的和，如下所示。

```
SET @sum=0;Query OK, 0 rows affected (0.05 sec)mysql> INSERT INTO tb_emp8    -> VALUES(1,'A',1,1000),(2,'B',1,500);Query OK, 2 rows affected (0.09 sec)Records: 2  Duplicates: 0  Warnings: 0mysql> SELECT @sum;+------+| @sum |+------+| 1500 |+------+1 row in set (0.03 sec)
```

### 13.6 创建 AFTER 类型触发器

在 test_db 数据库中，数据表 tb_emp6 和 tb_emp7 都为员工信息表，包含 id、name、deptId 和 salary 字段，数据表 tb_emp6 和 tb_emp7 的表结构如下所示。

```
mysql> SELECT * FROM tb_emp6;Empty set (0.07 sec)mysql> SELECT * FROM tb_emp7;Empty set (0.03 sec)mysql> DESC tb_emp6;+--------+-------------+------+-----+---------+-------+| Field  | Type        | Null | Key | Default | Extra |+--------+-------------+------+-----+---------+-------+| id     | int(11)     | NO   | PRI | NULL    |       || name   | varchar(25) | YES  |     | NULL    |       || deptId | int(11)     | YES  | MUL | NULL    |       || salary | float       | YES  |     | NULL    |       |+--------+-------------+------+-----+---------+-------+4 rows in set (0.00 sec)mysql> DESC tb_emp7;+--------+-------------+------+-----+---------+-------+| Field  | Type        | Null | Key | Default | Extra |+--------+-------------+------+-----+---------+-------+| id     | int(11)     | NO   | PRI | NULL    |       || name   | varchar(25) | YES  |     | NULL    |       || deptId | int(11)     | YES  |     | NULL    |       || salary | float       | YES  |     | 0       |       |+--------+-------------+------+-----+---------+-------+4 rows in set (0.04 sec)
```

【实例 2】创建一个名为 double_salary 的触发器，触发的条件是向数据表 tb_emp6 中插入数据之后，再向数据表 tb_emp7 中插入相同的数据，并且 salary 为 tb_emp6 中新插入的 salary 字段值的 2 倍。输入的 SQL 语句和执行过程如下所示。

```
mysql> CREATE TRIGGER double_salary    -> AFTER INSERT ON tb_emp6    -> FOR EACH ROW    -> INSERT INTO tb_emp7    -> VALUES (NEW.id,NEW.name,deptId,2*NEW.salary);Query OK, 0 rows affected (0.25 sec)
```

触发器 double_salary 创建完成之后，向表 tb_emp6 中插入记录时，同时向表 tb_emp7 中插入相同的记录，并且 salary 字段为 tb_emp6 中 salary 字段值的 2 倍，如下所示。

```
mysql> INSERT INTO tb_emp6    -> VALUES (1,'A',1,1000),(2,'B',1,500);Query OK, 2 rows affected (0.09 sec)Records: 2  Duplicates: 0  Warnings: 0mysql> SELECT * FROM tb_emp6;+----+------+--------+--------+| id | name | deptId | salary |+----+------+--------+--------+|  1 | A    |      1 |   1000 ||  2 | B    |      1 |    500 |+----+------+--------+--------+3 rows in set (0.04 sec)mysql> SELECT * FROM tb_emp7;+----+------+--------+--------+| id | name | deptId | salary |+----+------+--------+--------+|  1 | A    |      1 |   2000 ||  2 | B    |      1 |   1000 |+----+------+--------+--------+2 rows in set (0.06 sec)
```

### 13.7 MySQL 查看触发器

查看触发器是指查看数据库中已经存在的触发器的定义、状态和语法信息等。MySQL 中查看触发器的方法包括 SHOW TRIGGERS 语句和查询 information_schema 数据库下的 triggers 数据表等。

#### 13.7.1 SHOW TRIGGERS语句查看触发器信息

在 MySQL 中，可以通过 SHOW TRIGGERS 语句来查看触发器的基本信息，语法格式如下：

SHOW TRIGGERS;

###### 示例 1

首先创建一个数据表 account，表中有两个字段，分别是 INT 类型的 accnum 和 DECIMAL 类型的 amount。SQL 语句和运行结果如下：

```
mysql> CREATE TABLE account(    -> accnum INT(4),    -> amount DECIMAL(10,2));Query OK, 0 rows affected (0.49 sec)
```

创建一个名为 trigupdate 的触发器，每次 account 表更新数据之后都向 myevent 数据表中插入一条数据。创建数据表 myevent 的 SQL 语句和运行结果如下：

```
mysql> CREATE TABLE myevent(    -> id INT(11) DEFAULT NULL,    -> evtname CHAR(20) DEFAULT NULL);Query OK, 0 rows affected (0.26 sec)
```

创建 trigupdate 触发器的 SQL 代码如下：

```
mysql> CREATE TRIGGER trigupdate AFTER UPDATE ON account    -> FOR EACH ROW INSERT INTO myevent VALUES(1,'after update');Query OK, 0 rows affected (0.15 sec)
```

使用 SHOW TRIGGERS 语句查看触发器（在 SHOW TRIGGERS 命令后添加`\G`，这样显示信息会比较有条理），SQL 语句和运行结果如下：

```
mysql> SHOW TRIGGERS \G*************************** 1. row ***************************             Trigger: trigupdate               Event: UPDATE               Table: account           Statement: INSERT INTO myevent VALUES(1,'after update')              Timing: AFTER             Created: 2020-02-24 14:07:15.08            sql_mode: STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION             Definer: root@localhostcharacter_set_client: gbkcollation_connection: gbk_chinese_ci  Database Collation: latin1_swedish_ci1 row in set (0.09 sec)
```

由运行结果可以看到触发器的基本信息。对以上显示信息的说明如下：

- Trigger 表示触发器的名称，在这里触发器的名称为 trigupdate；
- Event 表示激活触发器的事件，这里的触发事件为更新操作 UPDATE；
- Table 表示激活触发器的操作对象表，这里为 account 表；
- Statement 表示触发器执行的操作，这里是向 myevent 数据表中插入一条数据；
- Timing 表示触发器触发的时间，这里为更新操作之后（AFTER）；
- 还有一些其他信息，比如触发器的创建时间、SQL 的模式、触发器的定义账户和字符集等，这里不再一一介绍。

SHOW TRIGGERS 语句用来查看当前创建的所有触发器的信息。因为该语句无法查询指定的触发器，所以在触发器较少的情况下，使用该语句会很方便。如果要查看特定触发器的信息或者数据库中触发器较多时，可以直接从 information_schema 数据库中的 triggers 数据表中查找。

#### 13.7.2在triggers表中查看触发器信息

在 MySQL 中，所有触发器的信息都存在 information_schema 数据库的 triggers 表中，可以通过查询命令 SELECT 来查看，具体的语法如下：

SELECT * FROM information_schema.triggers WHERE trigger_name= ‘触发器名’;

其中，`'触发器名'`用来指定要查看的触发器的名称，需要用单引号引起来。这种方式可以查询指定的触发器，使用起来更加方便、灵活。

###### 示例 2

下面使用 SELECT 命令查看 trigupdate 触发器，SQL 语句如下：

SELECT * FROM information_schema.triggers WHERE TRIGGER_NAME= ‘trigupdate’\G

上述命令通过 WHERE 来指定需要查看的触发器的名称，运行结果如下：

```
mysql> SELECT * FROM information_schema.triggers WHERE TRIGGER_NAME= 'trigupdate'\G*************************** 1. row ***************************           TRIGGER_CATALOG: def            TRIGGER_SCHEMA: test              TRIGGER_NAME: trigupdate        EVENT_MANIPULATION: UPDATE      EVENT_OBJECT_CATALOG: def       EVENT_OBJECT_SCHEMA: test        EVENT_OBJECT_TABLE: account              ACTION_ORDER: 1          ACTION_CONDITION: NULL          ACTION_STATEMENT: INSERT INTO myevent VALUES(1,'after update')        ACTION_ORIENTATION: ROW             ACTION_TIMING: AFTERACTION_REFERENCE_OLD_TABLE: NULLACTION_REFERENCE_NEW_TABLE: NULL  ACTION_REFERENCE_OLD_ROW: OLD  ACTION_REFERENCE_NEW_ROW: NEW                   CREATED: 2020-02-24 16:07:15.08                  SQL_MODE: STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION                   DEFINER: root@localhost      CHARACTER_SET_CLIENT: gbk      COLLATION_CONNECTION: gbk_chinese_ci        DATABASE_COLLATION: latin1_swedish_ci1 row in set (0.22 sec)
```

由运行结果可以看到触发器的详细信息。对以上显示信息的说明如下：

- TRIGGER_SCHEMA 表示触发器所在的数据库；
- TRIGGER_NAME 表示触发器的名称；
- EVENT_OBJECT_TABLE 表示在哪个数据表上触发；
- ACTION_STATEMENT 表示触发器触发的时候执行的具体操作；
- ACTION_ORIENTATION 的值为 ROW，表示在每条记录上都触发；
- ACTION_TIMING 表示触发的时刻是 AFTER；
- 还有一些其他信息，比如触发器的创建时间、SQL 的模式、触发器的定义账户和字符集等，这里不再一一介绍。

上述 SQL 语句也可以不指定触发器名称，这样将查看所有的触发器，SQL 语句如下：

```
SELECT * FROM information_schema.triggers \G
```

这个语句会显示 triggers 数据表中所有的触发器信息。

### 13.8 修改和删除触发器(DROP TRIGGER)

语法格式如下：

DROP TRIGGER [ IF EXISTS ] [数据库名] <触发器名>

语法说明如下：

#### 13.8.1 触发器名

要删除的触发器名称。

#### 13.8.2 数据库名

可选项。指定触发器所在的数据库的名称。若没有指定，则为当前默认的数据库。

#### 13.8.3 权限

执行 DROP TRIGGER 语句需要 SUPER 权限。

#### 13.8.4 IF EXISTS

可选项。避免在没有触发器的情况下删除触发器。

> 注意：删除一个表的同时，也会自动删除该表上的触发器。另外，触发器不能更新或覆盖，为了修改一个触发器，必须先删除它，再重新创建。

#### 13.8.5删除触发器

使用 DROP TRIGGER 语句可以删除 MySQL 中已经定义的触发器。

【实例】删除 double_salary 触发器，输入的 SQL 语句和执行过程如下所示。

```
mysql> DROP TRIGGER double_salary;Query OK, 0 rows affected (0.03 sec)
```

删除 double_salary 触发器后，再次向数据表 tb_emp6 中插入记录时，数据表 tb_emp7 的数据不再发生变化，如下所示。

```
mysql> INSERT INTO tb_emp6    -> VALUES (3,'C',1,200);Query OK, 1 row affected (0.09 sec)mysql> SELECT * FROM tb_emp6;+----+------+--------+--------+| id | name | deptId | salary |+----+------+--------+--------+|  1 | A    |      1 |   1000 ||  2 | B    |      1 |    500 ||  3 | C    |      1 |    200 |+----+------+--------+--------+3 rows in set (0.00 sec)mysql> SELECT * FROM tb_emp7;+----+------+--------+--------+| id | name | deptId | salary |+----+------+--------+--------+|  1 | A    |      1 |   2000 ||  2 | B    |      1 |   1000 |+----+------+--------+--------+2 rows in set (0.00 sec)
```

## 十四 DCL(Data Control Language)

### 14.1 回顾

- DDL : create / alter / drop
- DML: insert / update / delete
- DQL : select/ show
- DCL: grant/revoke

我们现在默认使用的都是 root 用户，超级管理员，拥有全部的权限。但是，一个公司里面的数据库服务器上面
可能同时运行着很多个项目的数据库。所以，我们应该可以根据不同的项目建立不同的用户，分配不同的权限来管
理和维护数据库。

### 

![20](https://www.kuangstudy.com/bbs/imgs/20.png)

注意 mysqld 是 MySQL 的主程序，服务器端。mysql 是 MySQL 的命令行工具，客户端

### 14.2 创建用户

#### 14.2.1 语法

```
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
```

#### 14.2.2 关键字说明

| 关键字   | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| ‘用户名’ | 将创建的用户名                                               |
| ‘主机名’ | 指定该用户子在哪个主机上可以登录,如果是本地用户可使用localhost,如果想让该用户可以从任意远程主机登录,可以使用通配符% |
| ‘密码’   | 该用户的登录密码,密码可以为空,如果为空则该用户可以不需要密码登录服务器 |

#### 14.2.3 具体操作

- 创建user1用户 ,只能在localhost这个服务器登录mysql服务器,密码为123;

```
create user 'user1'@'localhost' identified by '123';
```

- 创建user2用户可以在如何电脑上登录mysql服务器,密码为123;

```
create user 'user2'@'%' identified by '123';
```

注意 创建的用户名都在mysql 数据库中的user 表中可以查看到, 密码经过了加密.

### 14.3 给用户授权

用户创建后,没有什么权限! 需要给用户授权

#### 14.3.1 语法:

```
GRANT 权限1,权限2 ... ON 数据库名.表民 to '用户名'@'主机名';
```

#### 14.3.2 关键字说明

| 关键字            | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| GRANT…ON…TO       | 授权关键字                                                   |
| 权限              | 授予用户的权限，如 CREATE、ALTER、SELECT、INSERT、UPDATE 等。如果要授予所有的权限则使用 ALL |
| 数据库名.表名     | 该用户可以操作哪个数据库的哪些表。如果要授予该用户对所有数据库和表的相应操作权限则可用 *表示，如* . * |
| ‘用户名’@’主机名’ | 给哪个用户授权,注:有2对单引号                                |

#### 14.3.3 具体操作:

1. 给user1 用户分配对mytest 这个数据库操作的权限: 创建表,修改表,插入记录,更新记录,查询

   ```
   grant create,alter,insert,update,select on mytest.* to 'user1'@'localhost';
   ```

   注意: 用户名和主机名要于上面创建的相同,要加单引号.

2. 给user2用户分配所有权限,对所有数据库的所有表

```
grant all on *.* to 'user2'@'%';
```

### 14.4 撤销授权

#### 14.4.1 语法

```
REVORK     权限1,权限2,...on 数据库.表名 revoke all on mytest.* from '用户名'@'主机名';
```

#### 14.4.2 关键字说明:

| 关键字            | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| REVOKE…ON…FROM    | 撤销授权的关键字                                             |
| 权限              | 用户的权限，如 CREATE、ALTER、SELECT、INSERT、UPDATE 等，所有的权限则使用 ALL |
| 数据库名.表名     | 对哪些数据库的哪些表，如果要取消该用户对所有数据库和表的操作权限则可用 *示，如* . * |
| ‘用户名’@’主机名’ | 给哪个用户撤销                                               |

#### 14.4.3 具体操作:

- 撤销user1用户对mytest 数据库所有表的操作的权限

  ```
  revoke all on mytest.* from 'user1'@'localhost';
  ```

  注：用户名和主机名要与创建时相同，各自要加上单引号

### 14.5 查看权限

#### 14.5.1 语法:

```
SHOW GRANTS FOR '用户名'@'主机名';
```

#### 14.5.2 具体操作:

- 查看user1用户的权限

  ```
  show grants for 'user1'@'localhost';show grants for 'user2'@'%';
  ```

  注：usage 是指连接（登陆）权限，建立一个用户，就会自动授予其 usage 权限（默认授予）。

### 14.6 删除用户

#### 14.6.1 语法

```
drop user '用户名'@'主机名';
```

#### 14.6.2 具体操作

- 删除 user2

```
drop user 'user2'@'%';
```

### 14.7 修改管理员密码

#### 14.7.1 语法

```
mysqladmin -uroot -p password 新密码
```

需要在未登录mysql的情况下操作,新密码不需要加上引号.

### 14.8 修改普通用户密码

#### 14.8.1 语法:

```
set password for '用户名'@'主机名'=password('新密码');
```

注意：需要在登陆 MySQL 的情况下操作，新密码要加单引号。

标签：*数据库*

版权声明：本文为博主原创文章，转载请附上原文出处链接和