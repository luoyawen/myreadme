# Linux下安装RabbitMQ并使用SpringBoot整合

[![img](https://thirdwx.qlogo.cn/mmopen/vi_32/Exib7z7EGuVCcWhHZDayzNlvzEPA65WxfPxiacqtibdAM8yOpe96sBlkr9ichW8hWOeP4occYj1xicicbrMJopklicwmg/132) 海贼王路飞1999 ](https://www.kuangstudy.com/user/87a032d587864173b4000c1922b13089)分类：[学习笔记](https://www.kuangstudy.com/bbs?cid=4) 浏览：2442 [评论：1](https://www.kuangstudy.com/bbs/1339551571985350658#comments)[收藏](javascript:void(0);)最后修改于： 2021/01/10 09:28:53

[展开目录+](javascript:void(0);)

[CSDN原文链接](https://csp1999.blog.csdn.net/article/details/111315851)

## 1. RabbitMQ简介

**RabbitMQ**是基于**AMQP**协议的一款消息管理系统，是部署最广泛的开源消息中间件，是最受欢迎的开源消息中间件之一：

- 官网： [http://www.rabbitmq.com/](https://www.kuangstudy.com/bbs/1339551571985350658)
- 官方教程：[http://www.rabbitmq.com/getstarted.html](https://www.kuangstudy.com/bbs/1339551571985350658)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201216214203782.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

> AMQP 协议：

**AMQP**（advanced message queuing protocol）在2003年时被提出，最早用于解决金融领不同平台之间的消息传递交互问题。顾名思义，AMQP是一种协议，更准确的说是一种binary wire-level protocol（链接协议）。这是其和JMS的本质差别，**AMQP**不从API层进行限定，而是直接定义网络交换的数据格式。这使得实现了**AMQP**的provider天然性就是跨平台的。以下是**AMQP**协议模型；

> 不同MQ的区别：

- **ActiveMQ**：ActiveMQ 是Apache出品，最流行的，能力强劲的开源消息总线。它是一个完全支持JMS规范的的消息中间件。丰富的API,多种集群架构模式让ActiveMQ在业界成为老牌的消息中间件,在中小型企业颇受欢迎!
- **Kafka**：Kafka是LinkedIn开源的分布式发布-订阅消息系统，目前归属于Apache顶级项目。Kafka主要特点是基于Pull的模式来处理消息消费，追求高吞吐量，一开始的目的就是用于日志收集和传输。0.8版本开始支持复制，不支持事务，对消息的重复、丢失、错误没有严格要求，适合产生大量数据的互联网服务的数据收集业务。
- **RocketMQ**：RocketMQ是阿里开源的消息中间件，它是纯Java开发，具有高吞吐量、高可用性、适合大规模分布式系统应用的特点。RocketMQ思路起源于Kafka，但并不是Kafka的一个Copy，它对消息的可靠传输及事务性做了优化，目前在阿里集团被广泛应用于交易、充值、流计算、消息推送、日志流式处理、binglog分发等场景。
- **RabbitMQ**：RabbitMQ是使用Erlang语言开发的开源消息队列系统，基于AMQP协议来实现。AMQP的主要特征是面向消息、队列、路由（包括点对点和发布/订阅）、可靠性、安全。AMQP协议更多用在企业系统内对数据一致性、稳定性和可靠性要求很高的场景，对性能和吞吐量的要求还在其次。

**总结**：**RabbitMQ**比**Kafka**可靠，**Kafka**更适合**IO**高吞吐的处理，一般应用在大数据日志处理或对实时性（少量延迟），可靠性（少量丢数据）要求稍低的场景使用，比如**ELK**日志收集。

## 2. Linux中下载和安装

### 2.1 下载地址

官网下载地址：[ https://www.rabbitmq.com/download.html](https://www.kuangstudy.com/bbs/1339551571985350658)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217095737113.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

**下载安装压缩包**：

百度云下载链接：[https://pan.baidu.com/s/17hZab0ZKZLtoi-6q14R27Q](https://www.kuangstudy.com/bbs/1339551571985350658)，提取码：`dwti`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217104941312.png)

**注意**：这里的安装包是**centos7**安装的包！

### 2.2 安装步骤

#### 2.2.1 将rabbitmq安装包上传到linux系统中

这里我使用XFTP工具上传，使用XShell 连接云服务器，上传位置为：=[/usr/local/src/software/rabbitMQ/](https://www.kuangstudy.com/bbs/1339551571985350658)，如果对应的文件夹不存在，自己创建一下！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217105701605.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

#### 2.2.2 安装Erlang依赖包

进入目标文件夹下：`cd /usr/local/src/software/rabbitMQ/`，并查看安装包信息：`ls -l`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217110024767.png)

执行安装**Erlang**依赖包：`rpm -ivh erlang-22.0.7-1.el7.x86_64.rpm`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217110208652.png)

执行安装**Erlang**内存管理的依赖包：`rpm -ivh socat-1.7.3.2-2.el7.x86_64.rpm`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217110335823.png)

#### 2.2.3 安装RabbitMQ安装包(需要联网)

执行安装**rabbitmq**：`rpm -ivh rabbitmq-server-3.7.18-1.el7.noarch.rpm`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217110451647.png)

#### 2.2.4 复制配置文件

执行命令：`cp /usr/share/doc/rabbitmq-server-3.7.18/rabbitmq.config.example /etc/rabbitmq/rabbitmq.config`，该命令是将**rabbitmq.config.example** 配置文件复制到[/etc/rabbitmq/](https://www.kuangstudy.com/bbs/1339551571985350658) 下，并该名为**rabbitmq.config**！

**注意**：如果不知道**rabbitmq.config.example** 配置文件的具体位置，可以使用命令：`find / -name rabbitmq.config.example`去查找其位置！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217111221613.png)

因为是从[/](https://www.kuangstudy.com/bbs/1339551571985350658)目录开始查找可能会延迟几秒！

#### 2.2.5 查看/修改配置文件

查看配置文件位置：`ll /etc/rabbitmq/`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217111340430.png)

修改配置文件：`vim /etc/rabbitmq/rabbitmq.config`

开放来宾账户权限**loopback_users**：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020121711173534.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

把注释开放，注意尾部的逗号也去掉：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217111850148.png)

## 3. 相关命令

### 3.2 开启web界面管理工具

命令：`rabbitmq-plugins enable rabbitmq_management`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217112132352.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

### 3.2 启动、停止命令

- 服务启动：`systemctl start rabbitmq-server`
- 服务重启：`systemctl restart rabbitmq-server`
- 服务关闭：`systemctl stop rabbitmq-server`
- 查看服务运行状态：`systemctl status rabbitmq-server`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217112711902.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

## 4. RabbitMQ可视化管理页面

### 4.1 开放端口

如果是本地虚拟机开放防火墙对应端口，如果是云服务器开放安全组端口：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020121711311930.png)

### 4.2 访问管理页面

浏览器访问：[http://服务器IP地址:15672](https://www.kuangstudy.com/bbs/1339551571985350658)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217113320200.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

用户名和密码默认是我们在配置文件中开启的来宾账户：**guest/guest**，输入账号密码后进入管理页：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217113503994.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

![a](https://img-blog.csdnimg.cn/20201217114059620.png)

有上图可知，为了之后使用代码整合**RabbitMq**，我们还需要在安全组中开放另外两个端口：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217114423242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

### 4.3 管理界面导航介绍

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217115333452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

- `connections：无论生产者还是消费者，都需要与RabbitMQ建立连接后才可以完成消息的生产和消费，在这里可以查看连接情况`
- `channels：通道，建立连接后，会形成通道，消息的投递获取依赖通道。`
- `Exchanges：交换机，用来实现消息的路由`
- `Queues：队列，即消息队列，消息存放在队列中，等待消费，消费后被移除队列。`

### 4.4 Admin用户和虚拟主机管理

#### 4.4.1 添加用户

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217120848733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

上面的Tags选项，其实是指定用户的角色，可选的有以下几个：

- `超级管理员(administrator)`

  可登陆管理控制台，可查看所有的信息，并且可以对用户，策略(policy)进行操作。

- `监控者(monitoring)`

  可登陆管理控制台，同时可以查看rabbitmq节点的相关信息(进程数，内存使用情况，磁盘使用情况等)

- `策略制定者(policymaker)`

  可登陆管理控制台, 同时可以对policy进行管理。但无法查看节点的相关信息(上图红框标识的部分)。

- `普通管理者(management)`

  仅可登陆管理控制台，无法看到节点信息，也无法对策略进行管理。

- `其他`

  无法登陆管理控制台，通常就是普通的生产者和消费者。

#### 4.4. 2 添加虚拟主机

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217121001456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

点击给虚拟主机leyou 设置访问用户以及其权限：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217121229900.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

接下来退出登录，并以leyou用户登录即可！

## 5. Java API 操作RabbitMQ

> 5 种消息模型：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217144947579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

- 简单消息模型：1个生产者 + 1个队列 + 1个消费者；
- 工作队列消息模型：1个生产者 + 1个队列 + 多个消费者，一条消息只能被消费一次；
- 订阅消息模型之 **fanout**：1个生产者 + 1个交换机 + 多个队列 + 多个消费者，一条消息可以被多个消费者消费；
- 订阅消息模型之**durect/router**：1个生产者 + 1个交换机 + 多个队列 + 多个消费者 ，**routingKey** ，一条消息发送给符合 **routingKey** 的队列；
- 订阅消息模型之**topic**：通配符，`#`：匹配一个或者多个 `*`：一个词；

------

- 官网教程地址：https://www.rabbitmq.com/getstarted.html
- 参考传智播客的Demo 案例：[代码案例](https://gitee.com/caoshipeng/my-demo-code/tree/master/itcast-rabbitmq)，基本上是参考官网的**quickStart**的，可以快速上手！

## 6. Spring Boot 整合RabbiMQ

**Spring AMQP** 官网地址：https://spring.io/projects/spring-amqp

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217145358632.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

**SpringBoot**整合官方文档地址：https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-messaging

### 6.1 pom.xml 引入依赖

```
<dependency>    <groupId>org.springframework.boot</groupId>    <artifactId>spring-boot-starter-amqp</artifactId></dependency>
```

### 6.2 application.yml 中进行配置

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217145947830.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

```
# Spring 相关配置spring:  # RabbitMQ 相关配置  rabbitmq:    # 主机ip地址    host: 8.xxx.xx.xx6    # 授权用户名    username: leyou    # 授权用户密码    password: leyou    # 授权虚拟主机名称    virtual-host: leyou
```

### 6.3 RabbitMQ 监听器(消息消费者)组件注入IOC

```
/** * RabbitMQ 监听器组件：相当于消费者 */@Componentpublic class Listener {    @RabbitListener(bindings = @QueueBinding(            // value = "spring.test.queue" 队列名称            // durable = "true" 队列消息持久化            value = @Queue(value = "spring.test.queue", durable = "true"),            // value = "spring.test.exchange" 交换机名称            // durable = "true" 交换机消息持久化            // ignoreDeclarationExceptions = "true" 忽略声明异常            // type = ExchangeTypes.TOPIC 交换机类型：TOPIC            exchange = @Exchange(                    value = "spring.test.exchange",                    ignoreDeclarationExceptions = "true",                    type = ExchangeTypes.TOPIC//                    durable = "true"            ),            // 消息接收的路由表达式            key = {"#.#"}))    public void listen(String msg) {        System.out.println("接收到消息：" + msg);    }}
```

### 6.4 Test 测试模拟消息生产者

```
@RunWith(SpringRunner.class)@SpringBootTest(classes = Application.class)public class MqDemo {    // 注入AmqpTemplate模板    @Autowired    private AmqpTemplate amqpTemplate;    @Test    public void testSend() throws InterruptedException {        String msg = "hello, Spring boot amqp";        // 通过AmqpTemplate模板发送消息        // 参数1：交换机        // 参数2：a.b是否符合监听组件(消息消费者)中定义的消息接收的路由表达式        // 参数3：消息内容        this.amqpTemplate.convertAndSend("spring.test.exchange","a.b", msg);        // 等待10秒后再结束        Thread.sleep(10000);    }}
```

### 6.5 执行测试

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201217150800130.png)

可以看到消息生产后，被消息监听器(消费者)接收成功！

标签：*RabbitMQ*

版权声明：本文为博主原创文章，转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1339551571985350658](https://www.kuangstudy.com/bbs/1339551571985350658)