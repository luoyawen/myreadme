# RabbitMQ 实战

[![img](https://thirdwx.qlogo.cn/mmopen/vi_32/DYAIOgq83erRW5T3BZtuAP9K5B9S36RFicyiccGsSSaRYtOxOjO0O6Q46Nt3P7wpwcIxQJpTNx8DoKOBtcV6kxKQ/132) 只是一个笑话 ](https://www.kuangstudy.com/user/7e7b74dede0740bb8a8aa1372ebae574)分类：[学习笔记](https://www.kuangstudy.com/bbs?cid=4) 浏览：62 [评论：0](https://www.kuangstudy.com/bbs/1354066373420060673#comments)[收藏](javascript:void(0);)最后修改于： 2021/01/26 21:59:15

[展开目录+](javascript:void(0);)

# RabbitMQ 实战教程

## 1.MQ引言

### 1.1 什么是MQ

`MQ`(Message Quene) : 翻译为 `消息队列`,通过典型的 `生产者`和`消费者`模型,生产者不断向消息队列中生产消息，消费者不断的从队列中获取消息。因为消息的生产和消费都是异步的，而且只关心消息的发送和接收，没有业务逻辑的侵入,轻松的实现系统间解耦。别名为 `消息中间件`通过利用高效可靠的消息传递机制进行平台无关的数据交流，并基于数据通信来进行分布式系统的集成。

### 1.2 MQ有哪些

当今市面上有很多主流的消息中间件，如老牌的`ActiveMQ`、`RabbitMQ`，炙手可热的`Kafka`，阿里巴巴自主开发`RocketMQ`等。

### 1.3 不同MQ特点

```
# 1.ActiveMQ        ActiveMQ 是Apache出品，最流行的，能力强劲的开源消息总线。它是一个完全支持JMS规范的的消息中间件。丰富的API,多种集群架构模式让ActiveMQ在业界成为老牌的消息中间件,在中小型企业颇受欢迎!# 2.Kafka        Kafka是LinkedIn开源的分布式发布-订阅消息系统，目前归属于Apache顶级项目。Kafka主要特点是基于Pull的模式来处理消息消费，        追求高吞吐量，一开始的目的就是用于日志收集和传输。0.8版本开始支持复制，不支持事务，对消息的重复、丢失、错误没有严格要求，        适合产生大量数据的互联网服务的数据收集业务。# 3.RocketMQ        RocketMQ是阿里开源的消息中间件，它是纯Java开发，具有高吞吐量、高可用性、适合大规模分布式系统应用的特点。RocketMQ思路起        源于Kafka，但并不是Kafka的一个Copy，它对消息的可靠传输及事务性做了优化，目前在阿里集团被广泛应用于交易、充值、流计算、消        息推送、日志流式处理、binglog分发等场景。# 4.RabbitMQ        RabbitMQ是使用Erlang语言开发的开源消息队列系统，基于AMQP协议来实现。AMQP的主要特征是面向消息、队列、路由（包括点对点和        发布/订阅）、可靠性、安全。AMQP协议更多用在企业系统内对数据一致性、稳定性和可靠性要求很高的场景，对性能和吞吐量的要求还在        其次。
```

> RabbitMQ比Kafka可靠，Kafka更适合IO高吞吐的处理，一般应用在大数据日志处理或对实时性（少量延迟），可靠性（少量丢数据）要求稍低的场景使用，比如ELK日志收集。

------

## 2.RabbitMQ 的引言

### 2.1 RabbitMQ

> 基于`AMQP`协议，erlang语言开发，是部署最广泛的开源消息中间件,是最受欢迎的开源消息中间件之一。

`官网`: https://www.rabbitmq.com/

`官方教程`: https://www.rabbitmq.com/#getstarted

```
 # AMQP 协议         AMQP（advanced message queuing protocol）`在2003年时被提出，最早用于解决金融领不同平台之间的消息传递交互问题。顾名思义，AMQP是一种协议，更准确的说是一种binary wire-level protocol（链接协议）。这是其和JMS的本质差别，AMQP不从API层进行限定，而是直接定义网络交换的数据格式。这使得实现了AMQP的provider天然性就是跨平台的。以下是AMQP协议模型:
```

### 2.2 RabbitMQ 的安装

#### 2.2.1 下载

`官网下载地址`: https://www.rabbitmq.com/download.html![image-20190925220115235](https://img2020.cnblogs.com/blog/1573169/202101/1573169-20210126200807705-68493283.png)

> `最新版本`: 3.7.18

#### " class="reference-link">2.2.2 下载的安装包![image-20190925220343521](https://img2020.cnblogs.com/blog/1573169/202101/1573169-20210126200807935-1859226761.png)

> `注意`:这里的安装包是centos7安装的包

#### 2.2.3 安装步骤

```
# 1.将rabbitmq安装包上传到linux系统中    erlang-22.0.7-1.el7.x86_64.rpm    rabbitmq-server-3.7.18-1.el7.noarch.rpm# 2.安装Erlang依赖包    rpm -ivh erlang-22.0.7-1.el7.x86_64.rpm# 3.安装RabbitMQ安装包(需要联网)    yum install -y rabbitmq-server-3.7.18-1.el7.noarch.rpm        注意:默认安装完成后配置文件模板在:/usr/share/doc/rabbitmq-server-3.7.18/rabbitmq.config.example目录中,需要                    将配置文件复制到/etc/rabbitmq/目录中,并修改名称为rabbitmq.config# 4.复制配置文件    cp /usr/share/doc/rabbitmq-server-3.7.18/rabbitmq.config.example /etc/rabbitmq/rabbitmq.config# 5.查看配置文件位置    ls /etc/rabbitmq/rabbitmq.config# 6.修改配置文件(参见下图:)    vim /etc/rabbitmq/rabbitmq.config
```

将上图中配置文件中红色部分去掉`%%`,以及最后的`,`逗号 修改为下图:

```
# 7.执行如下命令,启动rabbitmq中的插件管理    rabbitmq-plugins enable rabbitmq_management    出现如下说明:        Enabling plugins on node rabbit@localhost:    rabbitmq_management    The following plugins have been configured:      rabbitmq_management      rabbitmq_management_agent      rabbitmq_web_dispatch    Applying plugin configuration to rabbit@localhost...    The following plugins have been enabled:      rabbitmq_management      rabbitmq_management_agent      rabbitmq_web_dispatch    set 3 plugins.    Offline change; changes will take effect at broker restart.# 8.启动RabbitMQ的服务    systemctl start rabbitmq-server    systemctl restart rabbitmq-server    systemctl stop rabbitmq-server# 9.查看服务状态(见下图:)    systemctl status rabbitmq-server  ● rabbitmq-server.service - RabbitMQ broker     Loaded: loaded (/usr/lib/systemd/system/rabbitmq-server.service; disabled; vendor preset: disabled)     Active: active (running) since 三 2019-09-25 22:26:35 CST; 7s ago   Main PID: 2904 (beam.smp)     Status: "Initialized"     CGroup: /system.slice/rabbitmq-server.service             ├─2904 /usr/lib64/erlang/erts-10.4.4/bin/beam.smp -W w -A 64 -MBas ageffcbf -MHas ageffcbf -             MBlmbcs...             ├─3220 erl_child_setup 32768             ├─3243 inet_gethost 4             └─3244 inet_gethost 4      .........
# 10.关闭防火墙服务    systemctl disable firewalld    Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.    Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.    systemctl stop firewalld   # 11.访问web管理界面    http://10.15.0.8:15672/
# 12.登录管理界面    username:  guest    password:  guest
```

------

## 

版权声明：本文为博主原创文章，转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1354066373420060673](https://www.kuangstudy.com/bbs/1354066373420060673)