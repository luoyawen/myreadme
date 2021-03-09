# Docker 安装 ElasticSearch + Head + Kibana



# O、前言

今天周末在家学习一下 `ElasticSearch` 。需要安装 ES、Head 和 Kibana 来做基础查询操作的熟悉。出于练习一下 `Docker` 的原因，环境安装都用了 Docker。把安装步骤和过程中的一些采坑记录一下。

# 一、拉取镜像

```
# 拉取 ElasticSearch镜像
docker pull elasticsearch:7.6.1
# 拉取 Head 镜像
docker pull mobz/elasticsearch-head:5
# 拉取 Kibana 镜像
docker pull kibana:7.6.1
```

# 二、启动容器

> 1、启动 ElasticSearch 容器

```
docker run -e ES_JAVA_OPTS=&quot;-Xms512m -Xmx512m&quot; -d --name elasticsearch  -p 9200:9200 -p 9300:9300 elasticsearch:7.6.1
```

**在启动 ElasticSearch 容器几秒后，容器自动退出了。`docker ps -a` 发现 ElasticSearch 容器状态为Exited (137)。**

查看日志发现如下报错：

```
docker logs -f 635b290dacbfERROR: Elasticsearch did not exit normally - check the logs at /usr/share/elasticsearch/logs/docker-cluster.log
```

需要加上参数`-e "discovery.type=single-node"`,完整的命令为：

```
docker run -e ES_JAVA_OPTS=&quot;-Xms512m -Xmx512m&quot; -d --name elasticsearch  -p 9200:9200 -p 9300:9300 elasticsearch:7.6.1
```

> 2、启动 head 容器

```
docker run -d -p 9100:9100 --name es_head docker.io/mobz/elasticsearch-head:5
```

浏览器访问 `127.0.0.1:9100`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210116225505921.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JpZ19kYXRhMQ==,size_16,color_FFFFFF,t_70)

这里可能会有**跨域访问**的问题

- 进入 ElasticSearch 容器内部，修改配置文件elasticsearch.yml

  ```
  # 查询运行容器 elasticsearch 的 iddocker ps -a   # 进入容器docker exec -it ******(容器id) /bin/bash# 进入config目录cd ./config# 编辑 elasticsearch.yml 文件vim elasticsearch.yml
  ```

- 在elasticsearch.yml中添加如下内容

  ```
  http.cors.enabled: truehttp.cors.allow-origin: &quot;*&quot;
  ```

- 重启 ElasticSearch容器

  ```
  docker restart elasticsearch
  ```

> 3、启动 kibana 容器

```
docker run -d -p 5601:5601 --name kibana --link elasticsearch:elasticsearch kibana:7.6.1
```

Kibana启动之后发现是全是英文，有些单词还是看不太懂，怎么办。不用担心，kibana 7.X已经为我们准备好了汉化选项。

中文包在 `/usr/share/kibana/node_modules/x-pack/plugins/translations/translations/zh-CN.jso`

只需要进入kibana容器，在配置文件 kibana.yml 中加入

```
i18n.locale: &quot;zh-CN&quot;
```

就可以了，汉化之后的效果如下图，这都能看懂了吧。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210116225442655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JpZ19kYXRhMQ==,size_16,color_FFFFFF,t_70)

# 三、使用 Head 和 Kibana

## 1、head 无法显示数据问题

通过 Kibana 插入数据后，在head的数据预览页面发现数据没有加载出来，打开检查，发现前端报错406。

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-5XgcuUWs-1610808335075)(/Users/yangdong/Library/Application Support/typora-user-images/image-20210116210224993.png)\]](https://img-blog.csdnimg.cn/20210116225522656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JpZ19kYXRhMQ==,size_16,color_FFFFFF,t_70)

> 解决方案

1. 进入head容器中，`docker exec -it es_head /bin/bash`

2. 进入 _site 目录 `cd _site/`

3. 编辑 vendor.js 共有两处

   - 6886行 `contentType: "application/x-www-form-urlencoded"` 修改为 `contentType: "application/json;charset=UTF-8"`

   - 7573行 `var inspectData = s.contentType === "application/x-www-form-urlencoded"` && 修改为 `var inspectData = s.contentType === "application/json;charset=UTF-8" &&`

     保存退出，**不需要重启容器**

4. 刷新浏览器，发现数据正确展示。

## 2、Head 容器中没有vim命令

- apt-get install vim

```
Reading package lists... DoneBuilding dependency treeReading state information... DoneE: Unable to locate package vim
```

- apt-get update
- apt-get install vim

## 3、安装ik分词插件

> 将压缩包移动到容器中

```
docker cp elasticsearch-analysis-ik-7.6.1.zip elasticsearch:/usr/share/elasticsearch/plugins
```

> 进入容器

```
docker exec -it elasticsearch /bin/bash
```

> 创建目录

```
mkdir /usr/share/elasticsearch/plugins/ik
```

> 将文件压缩包移动到ik中

```
mv /usr/share/elasticsearch/plugins/elasticsearch-analysis-ik-7.6.1.zip /usr/share/elasticsearch/plugins/ik
```

> 进入目录

```
cd /usr/share/elasticsearch/plugins/ik
```

> 解压

```
unzip elasticsearch-analysis-ik-7.6.1.zip
```

> 删除压缩包

```
rm -rf elasticsearch-analysis-ik-7.6.1.zip
```

> 退出并重启镜像

```
exitdocker restart elasticsearch
```

# 四、总结

到此，就可以愉快的练习ES的基础操作了。

说实话，ES的的增删改查方法真的是多，不好好练习，结合实际需求，还真的不容易记住，灵活运用更是不容易。