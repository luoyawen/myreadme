# Linux环境下ElasticSearch的安装与使用(SpringBoot整合云服务器上的ElasticSearch)

 海贼王路飞1999 ](https://www.kuangstudy.com/user/87a032d587864173b4000c1922b13089)分类：[学习笔记](https://www.kuangstudy.com/bbs?cid=4) 浏览：2426 [评论：5](https://www.kuangstudy.com/bbs/1333581359054131201#comments)[收藏](javascript:void(0);)最后修改于： 2021/01/10 22:36:41

[展开目录+](javascript:void(0);)

[我的CSDN原文链接](https://csp1999.blog.csdn.net/article/details/110392198)

## 0. Elaticsearch 简介

**Elaticsearch**，简称为**ES**，**ES**是一个开源的**高扩展的分布式全文检索引擎**，它可以近乎**实时的存储、检索数据**；本身扩展性很好，可以扩展到上百台服务器，处理 PB 级别（大数据时代）的数据。**ES**由 Java 语言开发并使用 **Lucene** 作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的 RESTFULL API 来隐藏 **Lucene** 的复杂性，从而让全文搜索变得简单。据国际权威的数据库产品评测机构 DB Engines 的统计，在2016 年1月，**ElasticSearch** 已超过 **Solr** 等，成为排名第一的搜索引擎类应用。

**ES 适用于全文搜索、结构化搜索、分析以及将这三者混合使用** ！

------

## 1. ES 的下载安装(Linux 下安装)

### 1.1 下载ES

**要求**：JDK版本最低为1.8 ！系统中已经配置好 JAVA 环境：https://blog.csdn.net/z69183787/article/details/78126238

- 官网地址：https://www.elastic.co/cn/
- 官方文档地址：https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html
- 下载地址：https://www.elastic.co/cn/downloads/past-releases/elasticsearch-7-6-1

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130143712228.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

### 1.2 安装ES

#### ES压缩包上传

我们将**ES** 的压缩包下载到本机电脑后，通过 XFTP 这个上传到服务器或者虚拟机指定文件夹位置：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130144138477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

我上传的位置是自己习惯的位置：[/usr/local/src/software](https://www.kuangstudy.com/bbs/1333581359054131201)

接下来使用 XShell 工具连接到服务器或者虚拟机(我使用的是阿里云服务器)

#### ES压缩包解压

解压命令：`tar -zxvf elasticsearch-7.6.1-linux-x86_64.tar.gz`

解压后的文件夹修改下名字，方便输入：`mv elasticsearch-7.6.1 elasticsearch7`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130145344888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

#### ES目录介绍

```
bin:下面存放着Es启动文件         elasticsearch.bat/elasticsearchconfig：配置目录data:数据目录jdk、lib：Java运行环境以及依赖包logs:日志目录modules、plugins：模块及插件目录，head插件可以存放在plugins目录下
```

#### ES相关配置

##### 基础配置

**ES** 本身其实也相当于是一个数据库，为此，我们在 elasticsearch7 文件夹下自己建一个 data 文件夹，用于存放数据：`mdkir data`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130145934187.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

进入 data 文件夹下，执行 `pwd` 命令拷贝下该文件夹的路径，下面配置要用到！

我的 data 路径：[/usr/local/src/software/elasticsearch7/data](https://www.kuangstudy.com/bbs/1333581359054131201)

修改配置文件**elasticsearch.yml**，我们进入elasticsearch7 这个文件夹下的config 文件夹，编辑 **elasticsearch.yml** 文件：`vim elasticsearch.yml`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130150348343.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130160519464.png)

下面 `wq` 保存退出，回到elasticsearch7 文件夹下，进入 logs 文件夹，`pwd` 查看该文件夹路径，并复制！

如：[/usr/local/src/software/elasticsearch7/logs](https://www.kuangstudy.com/bbs/1333581359054131201)，然后重新到 config 目录下编辑**elasticsearch.yml** 文件：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130150838382.png)

#### 跨域配置

**在config目录下的elasticsearch.yml文件末尾添加跨域允许：**

```
# 跨域问题http.cors.enabled: truehttp.cors.allow-origin: "*"http.cors.allow-headers: Authorization,X-Requested-With,Content-Length,Content-Type
```

具体配置流程还可以参考这篇文章：https://blog.csdn.net/weixin_43019282/article/details/105352982

## 2. ES 的分词器插件

> ES 自己默认带有分词器，但是支持的是英文分词，所以我们要安装一个可以对中文分词的插件

### 2.1 IK 分词器下载

IK 分词器 Github 地址：https://github.com/medcl/elasticsearch-analysis-ik

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130151441806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

**注意** ：IK 分词器的版本需要严格对应 ES的版本，所以我都使用的是 7.6.1 这个版本，IK 7.6.1下载地址https://github.com/medcl/elasticsearch-analysis-ik/releases/tag/v7.6.1

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130151745364.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

同样的，下载好后，我们使用XFTP 这个工具将其上传到服务器或者虚拟机指定文件夹，我个人还是习惯放到：

[/usr/local/src/software](https://www.kuangstudy.com/usr/local/src/software) 这个文件夹下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130151924942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

然后我们就可以使用Xshell 对其进行操作：

### 2.2 IK 分词器压缩包解压到ES 插件文件夹下

- 首先我们进入 elasticsearch7 文件夹下的 plugins 文件夹下，新建一个 ik 文件夹，用于存放 IK 分词器插件：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130152400620.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

ik 文件夹的路径为 [/usr/local/src/software/elasticsearch7/plugins/ik](https://www.kuangstudy.com/bbs/1333581359054131201)

- 然后我们在 software 文件夹下，将 IK 分词器的压缩包解压到 ik 文件中去，命令为：`unzip elasticsearch-analysis-ik-7.6.1.zip -d /usr/local/src/software/elasticsearch7/plugins/ik`，如果出现以下问题：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130152726743.png)

需要先下载 **unzip** 命令：`yum install unzip zip`，然后再执行解压命令：`unzip elasticsearch-analysis-ik-7.6.1.zip -d /usr/local/src/software/elasticsearch7/plugins/ik`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130152935199.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

然后我们到 ik 目录下检查是否解压成功：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130153051589.png)

### 2.4 IK 分词器的配置文件

#### 默认分词配置

进入ik 目录下的 config 目录：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130153550761.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

进入查看下 **extra_main.doc** 配置文件：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130153654909.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

可以看出，汉字相关的分词配置都是在配置文件中一个一个的枚举出来的！

#### 扩展分词配置

如果新出一些网络热词，我们想对其进行分词配置，我们需要到 ik 文件夹 中 config 文件夹下的**IKAnalyzer.cfg.xml** 去配置：`vim IKAnalyzer.cfg.xml`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130154108533.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

如果需要加一些额外的网络新词，可以把这些词放到 自己新建的 ext_dict 文件中，每个词直接都要换行，就像**extra_main.doc** 文件中的格式一样！

## 3. 启动与关闭ES

> 为了测试请求方便，我们需要下载 Postman 来模拟请求发送：[Postman 官网](https://www.postman.com/)，这个软件开发中很常用，我这里就不再赘述下载和使用步骤了！当然不一定非要使用Postman 模拟请求，其他类似的工具或者命令行都可以模拟，根据个人习惯选择把！

### 3.1 ES 服务启动与关闭

进入bin 目录，执行命令：`./elasticsearch` 运行**ES** 服务！后台运行命令：`./elasticsearch -d`

关闭ES 服务：`ps -ef|grep elastic` 查看进程，并使用 `kill -9 进程id` 来结束进程!

#### 可能出现问题一

在服务器上跑 **ES** 如果启动时出现以下错误：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201201090754617.png)

这种情况说明，内存不够了，服务器(学生机1核2g)内存较小，不足以启动ES服务，因为ES 默认启动内存大小就要求2g！

##### 解决方案一

可以修改 config 下的 jvm.options 配置文件，将运行大小 2g 修改为 1g(还是不行的话，再小设置到256m)：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130164851993.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

##### 解决方案二

##### 使用top命令查看占用内存多的进程将其结束：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130164600216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

**踩坑**，因为我这个学生机上有正在运行的 java 项目，包括 zookeeper 和 kafka等等服务，所以我换台学生机测试

#### 可能出现问题二

如果出现下面这种问题：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130165307178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

说明不能以root 用户去启动 **ES** 服务！

##### 解决方案

- 新建一个用户elasticsearch，命令： `adduser elasticsearch`

- 在software 目录下 赋予 elasticsearch7 这个文件夹的权限给 elasticsearch 用户，命令：`chown -R elasticsearch elasticsearch7`

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130170009613.png)

- `su elasticsearch` 命令，切换到 elasticsearch 用户，并重新到 bin 目录下执行 ES服务

#### 可能出现问题三

```
ERROR: bootstrap checks failedmax virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

##### 解决方案

- 修改配置sysctl.conf
  `vim /etc/sysctl.conf`
- 在最后一行添加下面配置：
  `vm.max_map_count=655360`
- 保存后退出并执行命令：
  `sysctl -p`

然后，重新启动 **elasticsearch**，即可启动成功。

#### 可能出现问题四

```
ERROR: [1] bootstrap checks failed[1]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
```

##### 解决方案

在`elasticsearch`的`config`目录下，修改`elasticsearch.yml`配置文件，将下面的配置加入到该配置文件中：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130173112959.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130173217296.png)

### 3.2 阿里云开放安全组端口

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130173651668.png)

### 3.3 测试访问ES服务

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130180713772.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

访问成功，说明ES启动成功了！

## 4. ES中基本概念

### 4.0关系型数据库和ES对比

| Relational DB      | Elasticsearch   |
| ------------------ | --------------- |
| 数据库(database)   | 索引(indices)   |
| 表(tables)         | 类型(types)     |
| 行(一条记录)(rows) | 文档(documents) |
| 字段(columns)      | 属性(fields)    |

### 4.1 接近实时(NRT Near Real Time )

**Elasticsearch**是一个接近实时的搜索平台。这意味着，从索引一个文档直到这个文档能够被搜索到有一个轻微的延迟(通常是1秒内)

### 4.2 索引(index)

**一个索引就是一个拥有几分相似特征的文档的集合**。比如说，你可以有一个客户数据的索引，另一个产品目录的索引，还有一个订单数据的索引。**一个索引由一个名字来标识(必须全部是小写字母的)**，**并且当我们要对这个索引中的文档进行索引、搜索、更新和删除的时候，都要使用到这个名字**。**索引类似于关系型数据库中Database 的概念**。在一个集群中，如果你想，可以定义任意多的索引。

### 4.3 类型(type)

**在一个索引中，你可以定义一种或多种类型**。一个类型是你的索引的一个逻辑上的分类/分区，其语义完全由你来定。通常，会为具有一组共同字段的文档定义一个类型。比如说，我们假设你运营一个博客平台并且将你所有的数 据存储到一个索引中。在这个索引中，你可以为用户数据定义一个类型，为博客数据定义另一个类型，当然，也可 以为评论数据定义另一个类型。**类型类似于关系型数据库中Table的概念**。

**NOTE: 在5.x版本以前可以在一个索引中定义多个类型,6.x之后版本也可以使用,但是不推荐,在7~8.x版本中彻底移除一个索引中创建多个类型**

### 4.4 映射(Mapping)

**Mapping**是ES中的一个很重要的内容，**它类似于传统关系型数据中table的schema，用于定义一个索引(index)中的类型(type)的数据的结构**。 在ES中，我们可以手动创建type(相当于table)和mapping(相关与schema),也可以采用默认创建方式。**在默认配置下，ES可以根据插入的数据自动地创建type及其mapping。 mapping中主要包括字段名、字段数据类型和字段索引类型**

### 4.5 文档(document)

**一个文档是一个可被索引的基础信息单元，类似于表中的一条记录。**比如，你可以拥有某一个员工的文档,也可以拥有某个商品的一个文档。文档以采用了轻量级的数据交换格式JSON(Javascript Object Notation)来表示。

## 5. ES 常用命令

### 5.1 索引操作

我们先使用命令行的方式 curl 发送请求：

#### 查看ES服务健康状态：

```
curl -X GET "ip地址:9200/_cat/health?v"
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130181527721.png)

#### 查看ES服务中的节点：

```
curl -X GET "ip地址:9200/_cat/nodes?v
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130181705215.png)

#### 查看ES服务中的所有索引：

```
curl -X GET "ip地址:9200/_cat/indices?v"
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020113018192840.png)

#### 新增索引：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020113018253869.png)

#### 删除索引：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130182710512.png)

从上面我们也能发现，每次使用 `curl` 去手动输入请求**ES** 服务不太方便，所以我们用 Postman 请求模拟工具来替代手动输入！

#### 新建索引：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130183438397.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

#### 新建文档并指定id：

```
PUT IP地址:9200/myindex/_doc/1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130190854806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

#### 根据id查询数据：

```
GET IP地址:9200/myindex/_doc/1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020113019101515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

#### 根据id修改数据：

```
PUT IP地址:9200/myindex/_doc/1
```

修改数据和新建一个文档一样，都是自定id 然后PUT 提交数据：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130191344913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

根据id删除数据：

```
DELETE IP地址:9200/myindex/_doc/1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130191504417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

### 5.2 索引搜索

为了搜索展示方便，先想ES 中添加一些数据：

```
PUT IP 地址:9200/myindex/_doc/1{    "title":"海贼王",    "content":"船长是路飞，副船长是索隆~"}PUT IP 地址:9200/myindex/_doc/2{    "title":"火影忍者",    "content":"七代火影是鸣人，八代火影是木叶丸~"}PUT IP 地址:9200/myindex/_doc/3{    "title":"一拳超人",    "content":"主角是埼玉，配角是杰诺斯~"}PUT IP 地址:9200/myindex/_doc/4{    "title":"进击的巨人",    "content":"人类最牛的是兵长，巨人最菜的是男主~"}PUT IP 地址:9200/myindex/_doc/5{    "title":"名侦探柯南",    "content":"男主是柯南，女主不确定~"}PUT IP 地址:9200/myindex/_doc/6{    "title":"海贼王路飞",    "content":"路飞是要成为海贼我的男人~"}PUT IP 地址:9200/myindex/_doc/7{    "title":"路飞的果实能力",    "content":"路飞是吃了橡胶果实的男人~"}
```

#### 搜索全部

```
GET IP地址:9200/myindex/_search
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130192839909.png)

返回结果：

```
{    "took": 1033,    "timed_out": false,    "_shards": {        "total": 1,        "successful": 1,        "skipped": 0,        "failed": 0    },    "hits": {        "total": {            "value": 7,            "relation": "eq"        },        "max_score": 1.0,        "hits": [            {                "_index": "myindex",                "_type": "_doc",                "_id": "5",                "_score": 1.0,                "_source": {                    "title": "名侦探柯南",                    "content": "男主是柯南，女主不确定~"                }            },            {                "_index": "myindex",                "_type": "_doc",                "_id": "4",                "_score": 1.0,                "_source": {                    "title": "进击的巨人",                    "content": "人类最牛的是兵长，巨人最菜的是男主~"                }            },            {                "_index": "myindex",                "_type": "_doc",                "_id": "3",                "_score": 1.0,                "_source": {                    "title": "一拳超人",                    "content": "主角是埼玉，配角是杰诺斯~"                }            },            {                "_index": "myindex",                "_type": "_doc",                "_id": "2",                "_score": 1.0,                "_source": {                    "title": "火影忍者",                    "content": "七代火影是鸣人，八代火影是木叶丸~"                }            },            {                "_index": "myindex",                "_type": "_doc",                "_id": "1",                "_score": 1.0,                "_source": {                    "title": "海贼王",                    "content": "船长是路飞，副船长是索隆~"                }            },            {                "_index": "myindex",                "_type": "_doc",                "_id": "6",                "_score": 1.0,                "_source": {                    "title": "海贼王路飞",                    "content": "路飞是要成为海贼我的男人~"                }            },            {                "_index": "myindex",                "_type": "_doc",                "_id": "7",                "_score": 1.0,                "_source": {                    "title": "路飞的果实能力",                    "content": "路飞是吃了橡胶果实的男人~"                }            }        ]    }}
```

#### 根据单个条件搜索

```
GET IP地址:9200/myindex/_search?q=title:海贼王
```

或者

```
GET IP地址:9200/myindex/_search?q=content:路飞
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130193338883.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

返回结果：

```
{    "took": 5,    "timed_out": false,    "_shards": {        "total": 1,        "successful": 1,        "skipped": 0,        "failed": 0    },    "hits": {        "total": {            "value": 3,            "relation": "eq"        },        "max_score": 1.7349373,        "hits": [            {                "_index": "myindex",                "_type": "_doc",                "_id": "1",                "_score": 1.7349373,                "_source": {                    "title": "海贼王",                    "content": "船长是路飞，副船长是索隆~"                }            },            {                "_index": "myindex",                "_type": "_doc",                "_id": "6",                "_score": 1.6770141,                "_source": {                    "title": "海贼王路飞",                    "content": "路飞是要成为海贼我的男人~"                }            },            {                "_index": "myindex",                "_type": "_doc",                "_id": "7",                "_score": 1.6770141,                "_source": {                    "title": "路飞的果实能力",                    "content": "路飞是吃了橡胶果实的男人~"                }            }        ]    }}
```

#### 根据多个条件搜索

```
GET IP地址:9200/myindex/_search
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130193741896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

```
{    "query":{        // 多个匹配        "multi_match":{            // 查询条件为 路飞            "query":"路飞",            // 从title 和 content 字段中匹配是否存在 路飞 的数据            "fields":["title","content"]        }    }}
```

返回结果：

```
{    "took": 32,    "timed_out": false,    "_shards": {        "total": 1,        "successful": 1,        "skipped": 0,        "failed": 0    },    "hits": {        "total": {            "value": 3,            "relation": "eq"        },        "max_score": 2.2700202,        "hits": [            {                "_index": "myindex",                "_type": "_doc",                "_id": "6",                "_score": 2.2700202,                "_source": {                    "title": "海贼王路飞",                    "content": "路飞是要成为海贼我的男人~"                }            },            {                "_index": "myindex",                "_type": "_doc",                "_id": "7",                "_score": 1.9412584,                "_source": {                    "title": "路飞的果实能力",                    "content": "路飞是吃了橡胶果实的男人~"                }            },            {                "_index": "myindex",                "_type": "_doc",                "_id": "1",                "_score": 1.7349373,                "_source": {                    "title": "海贼王",                    "content": "船长是路飞，副船长是索隆~"                }            }        ]    }}
```

如果要了解更多命令，可以参考这篇文章：https://blog.csdn.net/vtopqx/article/details/105998990

## 6. SpringBoot 整合 ES

### 6.1 引入依赖

**pom.xml** 中引入依赖

```
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-elasticsearch --><dependency>    <groupId>org.springframework.boot</groupId>    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>    <version>2.2.5.RELEASE</version></dependency>
```

### 6.2 添加 ES 相关配置

**application.yml** 中配置

```
# spring 相关配置spring:  # elasticsearch 配置  elasticsearch:    rest:      # ip是服务器ip地址      uris: http://8.xxx.xx.45:9200      # 我没有设置账号密码,所以下面可以不配置      #username:      #password:
```

### 6.3 相关注解介绍

> ES 几个常用注解

- ```
  @Document
  ```

  ：声明索引库配置

  - `indexName`：索引库名称
  - `type`：映射类型。如果未设置，则使用小写的类的简单名称。（**从版本4.0开始不推荐使用**）
  - `shards`：分片数量，默认 **5**
  - `replicas`：副本数量，默认 **1**

- `@Id`：声明实体类的id

- ```
  @Field
  ```

  ：声明字段属性

  - `type`：字段的数据类型
  - `analyzer`：指定在存储时候使用的分词器类型
  - `searchAnalyzer`：指定在搜索时候使用的分词器类型
  - `index`：是否创建索引

### 6.4 实体类

```
/** * @Auther: csp1999 * @Date: 2020/11/24/8:55 * @Description: 帖子实体类 */// Lombok 相关注解@Data@AllArgsConstructor@NoArgsConstructor@Accessors(chain = true)@ToString// ES 相关注解@Document(indexName = "discusspost",shards = 5,replicas = 1)// discusspost 必须全小写public class DiscussPost {    /**     * 主键id     */    @Id    private int id;    /**     * 用户主键id     */    @Field(type = FieldType.Long)    private int userId;    /**     * 帖子标题     */    @Field(type = FieldType.Text,analyzer = "ik_max_word",searchAnalyzer = "ik_smart")    private String title;    /**     * 帖子内容     */    @Field(type = FieldType.Text,analyzer = "ik_max_word",searchAnalyzer = "ik_smart")    private String content;    /**     * 帖子类型     * 0-普通; 1-置顶;     */    @Field(type = FieldType.Integer)    private int type;    /**     * 帖子状态     * 0-正常; 1-精华; 2-拉黑;     */    @Field(type = FieldType.Integer)    private int status;    /**     * 帖子创建日期     */    @Field(type = FieldType.Date, format = DateFormat.custom,pattern = "yyyy-MM-dd")    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern ="yyyy-MM-dd",timezone="GMT+8")    private Date createTime;    /**     * 帖子评论数量     */    @Field(type = FieldType.Integer)    private int commentCount;    /**     * 帖子得分     */    @Field(type = FieldType.Float)    private double score;}
```

### 6.5 Repository 接口

```
/** * @Auther: csp1999 * @Date: 2020/11/30/21:01 * @Description: 帖子相关的Repository */@Repositorypublic interface DiscussPostRepository extends ElasticsearchRepository<DiscussPost,Integer> {}
```

### 执行测试

```
/** * @Auther: csp1999 * @Date: 2020/11/30/21:04 * @Description: ES 测试 */@SpringBootTestpublic class ElasticSearchTest {    /**     * 从mysql数据中获取数据     */    @Autowired    private DiscussPostMapper discussPostMapper;    /**     * 注入 DiscussPostRepository     */    @Autowired    private DiscussPostRepository discussPostRepository;    /**     * 有些情况 DiscussPostRepository 处理不了     * 所以需要额外的 ElasticsearchRestTemplate     */    @Autowired    private ElasticsearchRestTemplate elasticsearchRestTemplate;    @Test    public void testInsert(){        // 从数据库查询 id 为217 282 283 284 的帖子存入 ES 中        discussPostRepository.save(discussPostMapper.selectDiscussPostById(217));        discussPostRepository.save(discussPostMapper.selectDiscussPostById(282));        discussPostRepository.save(discussPostMapper.selectDiscussPostById(283));        discussPostRepository.save(discussPostMapper.selectDiscussPostById(284));    }}
```

运行测试代码，如果执行成功后，使用Postman 测试查看 ES 中的索引库内容：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130214524121.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

可以看出，索引库已经创建好了！

我们来查一些数据内容：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020113021491421.png)

结果为：

```
{    "took": 249,    "timed_out": false,    "_shards": {        "total": 6,        "successful": 6,        "skipped": 0,        "failed": 0    },    "hits": {        "total": {            "value": 4,            "relation": "eq"        },        "max_score": 1.0,        "hits": [            {                "_index": "discusspost",                "_type": "_doc",                "_id": "282",                "_score": 1.0,                "_source": {                    "_class": "com.haust.community.pojo.DiscussPost",                    "id": 282,                    "userId": 152,                    "title": "哈哈***哈哈",                    "content": "***哈哈",                    "type": 0,                    "status": 0,                    "createTime": "2020-11-25",                    "commentCount": 1,                    "score": 0.0                }            },            {                "_index": "discusspost",                "_type": "_doc",                "_id": "284",                "_score": 1.0,                "_source": {                    "_class": "com.haust.community.pojo.DiscussPost",                    "id": 284,                    "userId": 153,                    "title": "海贼王我当定了！",                    "content": "海贼王我当定了！",                    "type": 0,                    "status": 0,                    "createTime": "2020-11-29",                    "commentCount": 2,                    "score": 0.0                }            },            {                "_index": "discusspost",                "_type": "_doc",                "_id": "217",                "_score": 1.0,                "_source": {                    "_class": "com.haust.community.pojo.DiscussPost",                    "id": 217,                    "userId": 103,                    "title": "互联网求职暖春计划",                    "content": "今年的就业形势，确实不容乐观。过了个年，仿佛跳水一般，整个讨论区哀鸿遍野！19届真的没人要了吗？！18届被优化真的没有出路了吗？！大家的“哀嚎”与“悲惨遭遇”牵动了每日潜伏于讨论区的牛客小哥哥小姐姐们的心，于是牛客决定：是时候为大家做点什么了！为了帮助大家度过“艰难”，牛客网特别联合60+家企业，开启互联网求职暖春计划，面向18届&19届，拯救0 offer！",                    "type": 0,                    "status": 0,                    "createTime": "2019-04-04",                    "commentCount": 0,                    "score": 0.0                }            },            {                "_index": "discusspost",                "_type": "_doc",                "_id": "283",                "_score": 1.0,                "_source": {                    "_class": "com.haust.community.pojo.DiscussPost",                    "id": 283,                    "userId": 152,                    "title": "海贼王我当定了！",                    "content": "Hello World!",                    "type": 0,                    "status": 0,                    "createTime": "2020-11-25",                    "commentCount": 3,                    "score": 0.0                }            }        ]    }}
```

### 6.6 SpringBoot 操作ES增删改

```
/** * @Auther: csp1999 * @Date: 2020/11/30/21:04 * @Description: ES 测试 */@SpringBootTestpublic class ElasticSearchTest {    /**     * 从mysql数据中获取数据     */    @Autowired    private DiscussPostMapper discussPostMapper;    /**     * 注入 DiscussPostRepository     */    @Autowired    private DiscussPostRepository discussPostRepository;    /**     * 有些情况 DiscussPostRepository 处理不了     * 所以需要额外的 ElasticsearchRestTemplate     */    @Autowired    private ElasticsearchRestTemplate elasticsearchRestTemplate;    /**     * 创建/更新索引     */    @Test    public void testCreateIndex() {        IndexOperations indexOperations = elasticsearchRestTemplate.indexOps(DiscussPost.class);        Document document = indexOperations.createMapping(DiscussPost.class);        //boolean bool = indexOperations.create(document);        boolean bool = indexOperations.putMapping(document);        System.out.println(bool?"成功":"失败");    }    /**     * 每次只插入一条     */    @Test    public void testInsert() {        // 从数据库查询 id 为217 282 283 284 的帖子存入 ES 中        discussPostRepository.save(discussPostMapper.selectDiscussPostById(217));        discussPostRepository.save(discussPostMapper.selectDiscussPostById(282));        discussPostRepository.save(discussPostMapper.selectDiscussPostById(283));        discussPostRepository.save(discussPostMapper.selectDiscussPostById(284));    }    /**     * 批量插入     */    @Test    public void testInsertAll() {        discussPostRepository.saveAll(discussPostMapper.selectDiscussPosts(101,0,100));        discussPostRepository.saveAll(discussPostMapper.selectDiscussPosts(102,0,100));        discussPostRepository.saveAll(discussPostMapper.selectDiscussPosts(103,0,100));        discussPostRepository.saveAll(discussPostMapper.selectDiscussPosts(152,0,100));        discussPostRepository.saveAll(discussPostMapper.selectDiscussPosts(153,0,100));    }    /**     * 测试修改     */    @Test    public void testUpdate() {        DiscussPost discussPost = discussPostMapper.selectDiscussPostById(203);        discussPost.setContent("海贼王，路飞当定了~");        discussPostRepository.save(discussPost);    }    /**     * 测试删除     */    @Test    public void testDelete() {        // discussRepository.deleteById(203);        discussPostRepository.deleteAll();//全部删除    }}
```

### 6.7 SpringBoot 操作ES 高亮搜索(核心)

效果如图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130223547480.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU5MTk4MA==,size_16,color_FFFFFF,t_70)

将从ES 中查询的内容，把搜索的关键字高亮显示借助 *标签：*

*参考代码：`@Testpublic void testSearchByTemplate() {    // 构建查询条件    NativeSearchQuery searchQuery = new NativeSearchQueryBuilder()            .withQuery(QueryBuilders.multiMatchQuery("互联网寒冬", "title", "content"))            .withSort(SortBuilders.fieldSort("type").order(SortOrder.DESC))// 按照帖子分类排序            .withSort(SortBuilders.fieldSort("score").order(SortOrder.DESC))// 按照帖子分数排序            .withSort(SortBuilders.fieldSort("createTime").order(SortOrder.DESC))// 按照帖子发布日期排序            .withPageable(PageRequest.of(0, 10))// 每页十条数据            .withHighlightFields(                    // 标题和内容中的匹配字段高亮展示                    new HighlightBuilder.Field("title").preTags("<em>").postTags("</em>"),                    new HighlightBuilder.Field("content").preTags("<em>").postTags("</em>")            ).build();    // 得到查询结果返回容纳指定内容对象的集合SearchHits    SearchHits<DiscussPost> searchHits = elasticsearchRestTemplate.search(searchQuery, DiscussPost.class);    // 设置一个需要返回的实体类集合    List<DiscussPost> discussPosts = new ArrayList<>();    // 遍历返回的内容进行处理    for (SearchHit<DiscussPost> searchHit : searchHits) {        // 高亮的内容        Map<String, List<String>> highlightFields = searchHit.getHighlightFields();        // 将高亮的内容填充到content中        searchHit.getContent().setTitle(highlightFields.get("title") == null ?                 searchHit.getContent().getTitle() : highlightFields.get("title").get(0));        searchHit.getContent().setTitle(highlightFields.get("content") == null ?                 searchHit.getContent().getContent() : highlightFields.get("content").get(0));        // 放到实体类中        discussPosts.add(searchHit.getContent());    }    // 输出结果    System.out.println(discussPosts.size());    for (DiscussPost discussPost : discussPosts) {        System.out.println(discussPost);    }}`参考相关文章：https://blog.csdn.net/qq_40885085/article/details/105024625https://blog.csdn.net/weixin_42385395/article/details/109315087https://blog.csdn.net/yj1499945/article/details/107726223https://www.cnblogs.com/hi3254014978/archive/2004/01/13/14055771.html*

标签：*ElasticSearch*

版权声明：本文为博主原创文章，转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1333581359054131201](https://www.kuangstudy.com/bbs/1333581359054131201)