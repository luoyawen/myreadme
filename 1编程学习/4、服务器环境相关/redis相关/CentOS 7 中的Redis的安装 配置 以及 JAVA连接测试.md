# CentOS 7 中的Redis的安装 配置 以及 JAVA连接测试

[![img](https://thirdwx.qlogo.cn/mmopen/vi_32/3ymOyN5u0ARBerceMI5WicunqbYamK6vfnLKfeyw6pic2sxjvWk6LdIyAiaCpzrcX3AJc2icVyzJbpJCW0jPbkficZA/132) 郑郑郑啊 ](https://www.kuangstudy.com/user/83b1eb1094584f9cb48b9e980f7ec8da)分类：[教程](https://www.kuangstudy.com/bbs?cid=3) 浏览：1910 [评论：0](https://www.kuangstudy.com/bbs/1329625628063862785#comments)[收藏](javascript:void(0);)最后修改于： 2021/01/07 14:32:20

[展开目录+](javascript:void(0);)

## CentOS 7 中的Redis的安装 配置 以及 JAVA连接测试

   最近重新配置redis时产生了 一些链接问题，不过网络上大家分享的方法都没有解决。所以给大家分享一下。跟着我的流程操作 应该是不会再出现连不上的问题。

## Linux下安装Redis

- 安装wget下载工具

```
yum -y install wget
```

- 使用wget工具下载相应版本的Redis：http://download.redis.io/releases/

```
wget http://download.redis.io/releases/redis-5.0.5.tar.gz
```

- 这里也可以用下好的redis压缩包
  **通过第三方工具拉进系统 再去解压，我使用的FinalShell**

- 检查是否有编译环境(更新或安装)

  ```
  yum install gcc-c++
  ```

- 解压下载的Redis软件,并移动到/use/local下

  ```
  tar -zxvf redis-5.0.5.tar.gzmv redis-5.0.5 /usr/local/
  ```

- 进入Redis解压文件夹编译Redis源码（注意没有 gcc ,make以后会出错，make完自愿 make install 测试 如果都是install 就没问题）

  ```
  cd /usr/local/redis-5.0.5/make
  ```

- 注意.redis.conf 配置文件默认在redis的根目录

- 启动redis，进入redis的src目录
  `java ./redis-server ../redis.conf`会
  这时候涉及到后台启动 还是前台启动，也就是会不会出现redis 的经典logo，只需要在redis.conf配置文件中更改，这边建议修改redis.conf配置留到最后一起修改，我只是在这里给大家提议一下

```
# 修改配置文件可以改变改变启动方式daemonize no\yes
```

- 查看Redis的进程信息

  ```
  ps -ef | grep  redis
  ```

  如果启动了，会显示如下：

  [root

  @localhost

   

  src]# ps -ef |grep redis

  root 58043 1 0 14:47 ? 00:00:00 ./redis-server *:6379

  root 58080 9737 0 14:47 pts/0 00:00:00 grep —color=auto redis

## 进入redis

```
cd到redis的src目录执行如下操作# 本机可简写为 ./redis-cli上述情况 如果存入中文，中文回显会乱码./redis-cli --raw (这样就不会乱码)
```

## Jedis介绍

   Redis不仅是使用命令来操作,现在基本上主流的语言都有客户端支持,比如java,C.C#、C++.php. Node.js. Go
等。
   在官方网站里列一些Java的客户端,有Jedis, Redisson、 Jredis、 JDBC-Redis、等其中官方推荐使用Jedis和
Redisson。在企业中用的最多的就是Jedis,下面我们就重点学习下Jedis。Jedis同样也是托管在github上,地址:https://github.com/xetorthio/jedis

==此时就需要开放我们CentOS系统的端口号！，重点，不开放6379端口 ping是通不了的 后台想去通过Jedis操作redis 链接不上==

## CentOS开放端口

- 1、运行命令：`firewall-cmd --get-active-zones`
- 2、执行如下命令命令：`firewall-cmd --zone=public --add-port=6379/tcp --permanent`
- 3、重启防火墙，运行命令：`firewall-cmd --reload`
- 4、查看端口号是否开启，运行命令：`firewall-cmd --query-port=6379/tcp`
  做完以上操作 就会把CentOS的防火墙打开6379的端口

## Java连接Redis

**导入jar包**

- jedis-2.9.0.jar
- commons-pool2-2.6.2.jar
  这里的jar包，网上都有

**java连接测试**

```
public class Testredis {    public static void main(String[] args) {          //建立连接        Jedis jedis = new Jedis("ip地址",6379);        System.out.println(jedis.ping());           //关闭资源        jedis.close();    }}
```

**单实例连接**

```
//这里使用 的是jedis-2.9.0.jarpublic class TestRedis {    public static void main(String[] args) {    //建立连接    Jedis jedis = new Jedis("ip地址",6379);    //获取数据    String test = jedis.get("test");    System.out.println("test = " + test);    //设置属性    jedis.set("test","hi，这是第一次设置的key值");    //获取数据    test = jedis.get("test");    System.out.println("test = " + test);    //关闭资源    jedis.close();    }}
```

**连接池连接**

```
public class TestRedis01 {        public static void main(String[] args) {        //1 获得连接池配置对象，设置配置项        JedisPoolConfig config = new JedisPoolConfig();        // 1.1 最大连接数        config.setMaxTotal(30);        //1.2 最大空闲连接数        config.setMaxIdle(10);        //获得连接池        JedisPool jedisPool = new JedisPool(config,"ip地址",6379);        Jedis jedis=null;        //3.获得核心对象        jedis = jedisPool.getResource();        //4.设置数据        jedis.set("name","这是连接池设置的key值");        //5.获得数据        String name = jedis.get("name");        System.out.println("name = " + name);        //6.关闭资源        jedis.close();    }}
```

**异常处理**
这个时候肯定有人会问怎么我报错了，你这教程有问题啊。

- 是因为redis默认bind 127.0.0.1，所以你会理所当然地想到去redis的配置文件redis.conf将“bind127.0.0.1”注释掉。认为这样就可以顺利访问了，其实还真不能解决，我们仍然会得到异常，异常的信息给我们提示了很多方法，其中有一个方法就是让我们将protected mode关闭掉。原来是redis默认开启了protected mode，保证只有主机才能访问到。所以正确解决jedis conneciton refused的解决方案如下：
      首先关掉redis-server，打开redis的配置文件redis.conf，将bind 127.0.0.1注释掉。
     这里别注释错了 因为配置文件往下翻的时候会有个已经注释了的bind 127.0.0.1，不要理，继续往下翻就会出现正主，果断注释掉。
     找到配置文件中protected-mode，默认protected mode yes，需要将其改为protected mode no
  此时在重新启动redis。用在运行java文件 就会发现 可以获取到redis的key了。
  ==注意，配置文件一定要正确，按照步骤来。如有问题可以私信我==

  这里如果redis那边正确的配置了，那么打印的`System.out.println(jedis.ping());`会打印`PONG`也就代表没问题了，之所以写这些 并不是安装redis会有问题，而是配置redis出问题会导致 java链接不上redis，网上很多教程是都不是使用的默认的防火墙，这里我使用的是默认的防火墙添加6379端口。

标签：*linux**redis**java*