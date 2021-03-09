### 单机Redis安装

##### 1.下载并解压Redis源码包

```
wget http://download.redis.io/releases/redis-5.0.4.tar.gztar -zxf redis-5.0.4.tar.gz
```

##### 2.编译Redis源代码

```
cd redis-5.0.4` make
```

##### 3.安装Redis到指定目录

```
make install PREFIX=/usr/jw/Redis/redis_1
```

##### 4.修改自定义端口并配置后台启动方式

```
#进入到redis配置文件目录cd /redis-5.0.4#修改redis配置文件 端口：port 8401 启动方式：daemonize yes vi redis.conf
```

##### 5.设置为开机启动

```
#在里面添加内容：/usr/jw/Redis/redis-5.0.4/redis.confvi /etc/rc.local
```

##### 6.启动Redis服务(带指定配置文件路径)

```
./redis-server /usr/jw/Redis/redis-5.0.4/redis.conf
```

##### 7.设置Redis服务密码

```
#通过redis-cli指定当前IP和端口。连接到redisredis-cli -h 127.0.0.1 -p 8401#连接成功后,设置密码config set requirepass gcredis
```

##### 8.外网访问服务器上的Redis

```
#在redis.conf中把bind 127.0.0.1注释掉#配置防火墙开放端口firewall-cmd --zone=public --add-port=8401/tcp --permanent#重启防火墙即时生效systemctl restart firewalld#通过redis-cli指定当前IP和端口,如果设置了密码需要加上-a后面输入Redis的密码。连接到Redisredis-cli -h 127.0.0.1 -p 8401 -a ***#关闭Redis服务保护模式。当此命令返回OK后, 就可以通过远程连接到当前服务器上的Redis服务config set protected-mode no
```

至此,在个人电脑上已经可以连接到Linux服务器上的Redis服务了。

![111](https://i.loli.net/2020/08/17/CW9YfSA6I2RoOHV.png)

##### 9.监测Redis服务是否存活

```
#监测服务是否存或ps -ef |grep redis#监测服务端口是否在监听netstat -lntp | grep 8401
```

##### 10.停止Redis服务

```
#使用redis自带的命令停止pkill redis#强行终止系统进程kill -9 pid
```

##### 11.监测当前服务器上是否有装Redis服务

```
#检测是否安装了客户端whereis redis-cli#检测是否安装了服务端whereis redis-server
```

### 集群Redis安装

##### 1.建立存储各个集群节点文件夹

```
#创建6个文件夹存放各个集群的节点mkdir redis_node_cluster_8402  redis_node_cluster_8403 redis_node_cluster_8404 redis_node_cluster_8405  redis_node_cluster_8406  redis_node_cluster_8407
```

##### 2.复制bin目录到各个集群节点目录中

```
#拷贝bin/redis.confcp -r  redis_1/bin/redis.conf  redis_node_cluster_8402cp -r  redis_1/bin/redis.conf  redis_node_cluster_8403cp -r  redis_1/bin/redis.conf  redis_node_cluster_8404cp -r  redis_1/bin/redis.conf  redis_node_cluster_8405cp -r  redis_1/bin/redis.conf  redis_node_cluster_8406cp -r  redis_1/bin/redis.conf  redis_node_cluster_8407
```

##### 3.修改各个节点的redis.conf配置

```
#修改每个集群节点下的redis.conf文件#关闭保护模式protected-mode no#配置当前节点在集群中的端口号port 8402#开启集群模式cluster-enabled yes#当前集群节点配置cluster-config-file nodes-8402.conf#日志文件名称logfile "8402.log"
```

##### 4.一键启动集群脚本

```
#创建一键启动脚本文件touch start_redis.sh#编辑一键启动内容vi start_redis.sh#下面是脚本里面的内容cd /usr/jw/Redis/redis_1/bin./redis-server /usr/jw/Redis/redis_node_cluster_8402/bin/redis.confcd /usr/jw/Redis/redis_1/bin./redis-server /usr/jw/Redis/redis_node_cluster_8403/bin/redis.confcd /usr/jw/Redis/redis_1/bin./redis-server /usr/jw/Redis/redis_node_cluster_8404/bin/redis.confcd /usr/jw/Redis/redis_1/bin./redis-server /usr/jw/Redis/redis_node_cluster_8405/bin/redis.confcd /usr/jw/Redis/redis_1/bin./redis-server /usr/jw/Redis/redis_node_cluster_8406/bin/redis.confcd /usr/jw/Redis/redis_1/bin./redis-server /usr/jw/Redis/redis_node_cluster_8407/bin/redis.conf
```

##### 5.设置脚本拥有可执行权限

```
chmod  +x  start_redis.sh#执行脚本,启动集群节点./start_redis.sh
```

**6.个集群节点已经启动,查看节点状态也是OK的**

![image-20200819135245675](https://i.loli.net/2020/08/18/6LdmDICHKvqFQ9X.png)

至此6个redis节点启动成功，接下来正式开启搭建集群，以上都是准备条件。

##### 7.安装Ruby

Redis集群的操作是通过Ruby脚本来完成的，因此我们需要安装Ruby相关的RPM包。

```
#安装curlsudo yum install curl#在/etc/hosts文件中配置host(下载rvm使用的,不然的话国内的服务器会连接不上下载rvm的地址)199.232.68.133  raw.githubusercontent.com#下载rvm前先请求拿到秘钥gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB#使用curl方式下载rvm（使用rvm去管理ruby）curl -sSL https://get.rvm.io | bash -s stable#下载完成后,刷新一下环境变量配置source ~/.bashrcsource ~/.bash_profile#使用rvm方式安装ruby(指定ruby版本)。需要等待挺久的rvm install 2.6.0#使用ruby -v验证是否安装成功ruby -v#如果版本和安装的不匹配,可以指定刚刚下载的ruby版本为默认版本rvm use 2.6.0 default
```

##### 8.开放集群端口和集群总线端口

```
#配置防火墙开发端口firewall-cmd --zone=public --add-port=8402/tcp --permanentfirewall-cmd --zone=public --add-port=8403/tcp --permanentfirewall-cmd --zone=public --add-port=8404/tcp --permanentfirewall-cmd --zone=public --add-port=8405/tcp --permanentfirewall-cmd --zone=public --add-port=8406/tcp --permanentfirewall-cmd --zone=public --add-port=8407/tcp --permanent#开放Redis服务的两个TCP端口。比如Redis客户端连接端口为8402，而Redis服务在集群中还有一个叫集群总线端口，其端口为客户端连接端口加上10000，即 8402 + 10000 = 16379。所以开放每个集群节点的客户端端口和集群总线端口才能成功创建集群firewall-cmd --zone=public --add-port=18402/tcp --permanentfirewall-cmd --zone=public --add-port=18403/tcp --permanentfirewall-cmd --zone=public --add-port=18404/tcp --permanentfirewall-cmd --zone=public --add-port=18405/tcp --permanentfirewall-cmd --zone=public --add-port=18406/tcp --permanentfirewall-cmd --zone=public --add-port=18407/tcp --permanent#重启防火墙即时生效systemctl restart firewalld
```

##### 9.创建Redis集群

```
#执行创建集群redis-cli --cluster create 127.0.0.1:8402 127.0.0.1:8403 127.0.0.1:8404 127.0.0.1:8405 127.0.0.1:8406 127.0.0.1:8407 --cluster-replicas 1
```

![image-20200819135245675](https://i.loli.net/2020/08/19/dDbqrNPCoYtkXhz.png)

集群创建成功,可以看到8402，8403，8404节点被分配为主节点。并且分配的hash槽是

8402-》 0 - 5460

8403-》 5461 - 10922

8404-》 10923 - 16383

其它三个节点为从节点。只做备份数据使用。

至此,已经可以使用第三方Redis可视化工具连接到我们刚才创建的Redis集群

![image-20200819141121096](https://i.loli.net/2020/08/19/8VJeAZj9OrEXsG1.png)

##### 10.查看Redis集群节点

```
#使用redis-cli进入到某一个集群节点中即可查看redis-cli -c -p 8402
```

![image-20200819140734879](https://i.loli.net/2020/08/19/sJI4dLO5cTHjKzq.png)

##### 11.查看集群信息

![image-20200819140829920](https://i.loli.net/2020/08/19/Zo4RxsKO3I9A2fz.png)

- `cluster_state`: `ok`状态表示集群可以正常接受查询请求。`fail` 状态表示，至少有一个哈希槽没有被绑定（说明有哈希槽没有被绑定到任意一个节点），或者在错误的状态（节点可以提供服务但是带有FAIL 标记），或者该节点无法联系到多数master节点。.
- `cluster_slots_assigned`: 已分配到集群节点的哈希槽数量（不是没有被绑定的数量）。16384个哈希槽全部被分配到集群节点是集群正常运行的必要条件.
- `cluster_slots_ok`: 哈希槽状态不是`FAIL` 和 `PFAIL` 的数量.
- `cluster_slots_pfail`: 哈希槽状态是 `PFAIL`的数量。只要哈希槽状态没有被升级到`FAIL`状态，这些哈希槽仍然可以被正常处理。`PFAIL`状态表示我们当前不能和节点进行交互，但这种状态只是临时的错误状态。
- `cluster_slots_fail`: 哈希槽状态是`FAIL`的数量。如果值不是0，那么集群节点将无法提供查询服务，除非`cluster-require-full-coverage`被设置为`no` .
- `cluster_known_nodes`: 集群中节点数量，包括处于`握手`状态还没有成为集群正式成员的节点.
- `cluster_size`: 至少包含一个哈希槽且能够提供服务的master节点数量.
- `cluster_current_epoch`: 集群本地`Current Epoch`变量的值。这个值在节点故障转移过程时有用，它总是递增和唯一的。
- `cluster_my_epoch`: 当前正在使用的节点的`Config Epoch`值. 这个是关联在本节点的版本值.
- `cluster_stats_messages_sent`: 通过node-to-node二进制总线发送的消息数量.
- `cluster_stats_messages_received`: 通过node-to-node二进制总线接收的消息数量.