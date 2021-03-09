# Redis的五种基本数据类型和使用场景

[![img](https://thirdwx.qlogo.cn/mmopen/vi_32/LyXoVuH1CSYOTiba6S4MW78ysIZGkp8RKicnmIbPFMvfcKr8D55b1PA8EiaX30SgibliacCjYVS9fbvKtdjhqyMeDVA/132) 路飞君的草帽 ](https://www.kuangstudy.com/user/a80ccd06baeb4c2fb23e29b26d65680e)分类：[学习笔记](https://www.kuangstudy.com/bbs?cid=4) 浏览：200 [评论：3](https://www.kuangstudy.com/bbs/1352824042272366593#comments)[收藏](javascript:void(0);)最后修改于： 2021/01/23 11:42:41

[展开目录+](javascript:void(0);)

### Redis是什么

##### 百度百科

（Remote Dictionary Server），远程字典服务。它是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。免费和开源！是当下最热门的技术之一，也被称之为结构化数据库。redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。

##### Redis的官网介绍

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 它支持多种类型的数据结构，如 字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询。 Redis 内置了 复制（replication），LUA脚本（Lua scripting），LRU驱动事件（LRU eviction），事务（transactions） 和不同级别的 磁盘持久化（persistence）， 并通过 Redis哨兵（Sentinel）和自动 分区（Cluster）提供高可用性（high availability）。

### Redis能干嘛？

1.内存存储、持久化、内存中是断电即失的、所以锁持久化很重要（rdb\aof）;
2.效率高用于告诉缓存。
3.发布订阅信息。
4.地图信息分析。
5.计数器（游览量），计时器。

### Redis的特性？（一些零散的知识点，都是比较有用的我才放上的）

1.多样的数据类型
2.持久化
3.集群
4.事务

Redis的端口号
6379

redis的配置文件
daemonize yes (修改启动方式为后台启动了)

启动redis服务
redis-server +redis.config
关闭
showdown

清空当前库flushdb 所有库flushall
查看key keys *

### Redis是单线程的

明白Redis是很快的，官方表示，Redis是基于内存操作的，CPU不是Redis性能瓶颈，瓶颈是根据机器（服务器）的内存和网络带宽，既然可以使用单线程来实现，就使用单线程了

### 为什么单线程还这么快？

Redis是C语言写的
官方提供的数据为10万+QPS,这个不比使用的key-value的Memecache差
1.误区：高性能的服务器不一定是多线程的
2.误区2：多线程（CPU上下文切换）的不一定比单线程的快。
CPU 》内存 》硬盘 熟读要有了解（GUC线程）

##### 核心解释：

`java redis是将所有的数据全部放到内存中的，如果使用的是多线程，CPU就会进行上下文的切换，这样会有耗时的操作，对与系统来说，如果没有上下文的切换效率是最高的， 多次读写都是在一个cpu上，在内存的情况下这就是最佳的解决方案`。

### 五种数据类型

###### 1.String类型

set key value (set name zhangshan)
get key (get name)
del key (del name)
incr key 将key中存储的数字值增一
incrby key 10 将key中存储的数字值增10点赞次数 ~~~~视频播放次数~~~~
decr key
decrby key

###### 2.hash类型

```
应用实例：存储java对象 对象==>json==>存放如hashhash特别适用于存储对象即把key 看成String key 和value的容器，也就是说把值看成一个map集合。
```

hset key field value 将哈希表key中的字段field的值设置为value.
hmset key field value … 同时将多个field-value（字段-值）对设置到哈希表key中。
hget key field 获取field字段中的值
hmget key field … 获取多个
hdel key field … 删除

###### 3.list类型（消息队列就是使用的redis中的这个基本数据类型）

```
  商品评论列表：思路: 在Redis中创建商品评论列表,用户发布商品评论，  将评论信息转成json存储到list中。用户在页面查询评论列表，  从redis中取出json数据展示到页面。
```

lpush key value…将一个或多个值插入到列表头部(左边)
rpush key value…将一个或多个值添加到列表尾部（右边）
lpop key…左边弹出一个相当于移出第一个
rpop key…右边弹出一个相当于移出最后一个

###### 4.set类型（是String类型的无序集合，成员唯一）

```
  场景应用：共同好友  可以通过sinter，比如A用户的所有好友的ID放到一个set集合，  B用户所有好友的ID放到一个集合，然后取他们的交集 sinter AID BID
```

sadd key value… 想集合中添加一个或多个成员
srem key valuesdiff key1 key2…. 移除集合中一个或多个成员
sdiff key1 key2 返回给定集合的差集
sunion key1 key2 返回给定集合的并集
sinter key1 key2 返回给定集合的交集

###### 5.zset类型

应用实例：商品销售排行榜
定义商品销售排行榜（sorted set集合），Key为items:sellsort，分数为商品销售量

```
--商品编号1001的销量是9，商品编号1002的销量是10ZADD items:sellsort 9 1001 10 1002        --商品编号1001的销量加1ZINCRBY items:sellsort 1 1001--商品销量前10名ZRANGE items:sellsort 0 -9 withscores
```

ZRANGE key start stop[WITHSCORES]获得排名在某个范围的元素列表
zadd key score value 增加元素
zrem key value 删除元素

`java 明天会更新redis剩下的内容，持久化，缓存击穿，订阅发布等...希望大家关注我`！

版权声明：本文为博主原创文章，转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1352824042272366593](https://www.kuangstudy.com/bbs/1352824042272366593)