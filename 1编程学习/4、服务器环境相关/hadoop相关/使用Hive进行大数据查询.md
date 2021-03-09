## 1 hive简介

Hive是基于Hadoop的一个数据仓库工具，避免去写MapReduce，提供类似SQL查询功能。（Hadoop是一个分布式系统的基础架构，用于开发并行计算的分布式程序。可以将大型任务分解成很多单个并行执行的任务，计算结果合并在一起，提高运行效率。）

Hive中查询数据使用Hive-SQL，和MySQL的语法最为接近。



```sql
 select * from aima_dwd.dwd_sr_delivery_project_data limit 10;--查询前10条数据
explain select * from aima_dwd.dwd_sr_delivery_project_data limit 10; --Hive的Explain命令，用于显示SQL查询的执行计划
```



### 1.1 hive查询文档介绍

hive查询的官网文档（网页语言为英文）：[https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL](https://link.zhihu.com/?target=https%3A//cwiki.apache.org/confluence/display/Hive/LanguageManual%2BDDL)

**语法概览**:

```sql
CREATE DATABASE/SCHEMA, TABLE, VIEW, FUNCTION, INDEX
DROP DATABASE/SCHEMA, TABLE, VIEW, INDEX
TRUNCATE TABLE
ALTER DATABASE/SCHEMA, TABLE, VIEW
DESCRIBE DATABASE/SCHEMA, table_name, view_name, materialized_view_name
```

## 2 hive中创建数据库、创建表、装载数据

hive中创建数据库：create database databaseNname

```sql
hive (default)> create database db1;
OK
Time taken: 0.202 seconds
```

切换数据库：use databaseNname

```sql
hive (default)> use test;
OK
Time taken: 0.055 seconds
```

创建表：create table (if not exists) tableNname (col1 type) row format delimited fields terminated by ‘\t’ collection items terminated by ‘\001’ stored as textfile

```sql
hive (db1)> create table weblogs
(   client_ip string,
    full_request_date string,
    day string,
    month string,
    ..., （篇幅限制省略一些字段名称）
    user_agent string)
row format delimited
fields terminated by ‘\t’;

OK
Time taken: 0.615 seconds
```

装载数据：load data local inpath ‘XXX’ into table tableName

```sql
hive (db1)> load data local inpath '/home/grid/weblogs_parse.txt' into table weblogs;

Loading data to table db1.weblogs
OK
Time taken: 2.946 seconds
```

## 3 hive查询

基本查询语法：

```sql
select column as alias 
  from tableName where column like ‘模糊匹配字符串’ 
  group by 分组字段 having count(分组字段)>n
  order by 排序字段
```

### 3.1 查询前几行limit

查询前几行记录：select * from tableName limit n;

```sql
hive (db1)> select * from weblogs limit 10;
OK
612.57.72.653  03/Jun/2012:09:12:23  -0500 03 Jun 6 2012 09 12 23 -0500 
GET /product/product2 200 0 /product/product2 Mozilla/4.0 (compa...（篇幅限制省略一些记录）
Time taken: 0.31 seconds, Fetched: 10 row(s)
```

### 3.2 使用运算符和内置函数

```sql
hive (db1)> select year from weblogs limit 1;
OK
year
2012
Time taken: 0.223 seconds, Fetched: 1 row(s)

hive (db1)> select cast(year as double) from weblogs limit 1;
OK
year
2012.0
Time taken: 0.297 seconds, Fetched: 1 row(s)

hive (db1)> select year + 0.0 from weblogs limit 1;
OK
_c0
2012.0
Time taken: 0.326 seconds, Fetched: 1 row(s)
```

### 3.3 表生成函数explode

explode可以行转列：

```sql
hive (db1)> select explode(split(user_agent,' ')) from weblogs limit 10;
OK
Mozilla/4.0
(compatible;
MSIE
7.0;
Windows
NT
5.1;
.NET
CLR
1.1.4322;
Time taken: 0.238 seconds, Fetched: 10 row(s)
```

### 3.4 if、case-when函数

if函数：IF(expr1,expr2,expr3)

case-when函数：CASE a WHEN b THEN c [ELSE f] END

```sql
hive (db1)> desc function if;  -- 使用desc functions 关键字查看函数文档。
OK
IF(expr1,expr2,expr3) - If expr1 is TRUE (expr1 <> 0 and expr1 <> NULL) then IF() returns expr2; 
  otherwise it returns expr3. IF() returns a numeric or string value, 
  depending on the context in which it is used.
Time taken: 0.056 seconds, Fetched: 1 row(s)

hive (db1)> desc function case;
OK
CASE a WHEN b THEN c [WHEN d THEN e]* [ELSE f] END - When a = b, returns c; 
  when a = d, return e; else return f
Time taken: 0.057 seconds, Fetched: 1 row(s)

hive (db1)> select if(year<2010,'before_2010','after_2010') from weblogs limit 10;
OK
after_2010
...
Time taken: 0.304 seconds, Fetched: 10 row(s)

hive (db1)> select case year when 2000 then 2000 else 1990 end from weblogs limit 10;
OK
1990
...
Time taken: 0.504 seconds, Fetched: 10 row(s)
```

### 3.5 子查询

```sql
hive (db1)> select * from(select * from weblogs limit 10) as t; 
OK
612.57.72.653 03/Jun/2012:09:12:23 -0500 03 Jun 6 2012 09 12 23 -0500 GET /produc...)
Time taken: 0.265 seconds, Fetched: 10 row(s)

hive (db1)> from(select * from weblogs limit 10) as t select *; 
OK
612.57.72.653 03/Jun/2012:09:12:23 -0500 03 Jun 6 2012 09 12 23 -0500 GET /product/p...9)
Time taken: 0.305 seconds, Fetched: 10 row(s)
```

### 3.6 聚合count、avg

聚合函数一般情况下会触发MapReduce过程，开始启动job和分布式查询。

```sql
hive (db1)> select avg(year) from weblogs;
Query ID = grid_20190613111237_b7078237-ab08-4266-8250-0990189f2dc0
Total jobs = 1
Launching Job 1 out of 1
Starting Job = job_1560304819522_0006, 
Tracking URL = http://master:8088/proxy/application_1560304819522_0006/
Kill Command = /home/grid/hadoop-2.7.6/bin/hadoop job -kill job_1560304819522_0006
...(省略部分过程)
OK
2012
Time taken: 253.561 seconds, Fetched: 1 row(s)
```

### 3.7 分组group by...having

待更新

### 3.8 连接 join...on

待更新

### 3.9 排序 orderBy

语法：局部排序sortBy，划分reducer的distributeBy，distribute加sort的简化版clusterBy

待更新

### 3.10 合并union

待更新

**UNION用于联合多个select语句的结果集，合并为一个独立的结果集，结果集去重。**

**UNION ALL也是用于联合多个select语句的结果集。但是不能消除重复行。现在hive只支持UNION ALL。**

**这里需要特别注意，每个select语句返回的列的数量和名字必须一样，同时字段类型必须完全匹配，否则会抛出语法错误。**

字段名称一样，并不是必须完全一样，比如下面这个例子：

**例一：字段名完全一样**

select a,b,c from t1

union all

select a,b,c from t2

**例二：字段名前面有表名不一致，其他一致**

select t1.a,t2.b,t2.c from t1

inner join t2 on t1.a = t2.a

union all

select t3.a,t4.b,t4.c from t3

inner join t4 on t3.a = t4.a

这两个例子都不报错

但

**例三：第一个查询第二个字段重命名为k，与第二个查询字段名不一样了，此时会报错**

select a,'' as k,c from t1

union all

select a,b,c from t2

会报编译错误

**编译错误：SemanticException The abstract syntax tree is null**