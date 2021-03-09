# Centos8.0编译安装稳定最新版的nginx

[![img](https://thirdwx.qlogo.cn/mmopen/vi_32/Vcdhv8zrIpgnxeYW8Oe65XoHVrdqAzeic7ibg4RofoqyConWbOoHaCxJYcrT6bLjMZ0SUcqo4CmFFGPcQGibEVFKA/132) 兮动人 ](https://www.kuangstudy.com/user/f41845a8a9f44b9f8210334cc869eb5d)分类：[运维](https://www.kuangstudy.com/bbs?cid=9) 浏览：506 [评论：0](https://www.kuangstudy.com/bbs/1354639866087895041#comments)[收藏](javascript:void(0);)最后修改于： 2021/01/28 11:58:07

- nginx有三个版本模式，有关详细介绍可以访问我以前写的这篇博文，https://blog.csdn.net/qq_41684621/article/details/101900843
- 下面介绍我安装最新稳定版的 nginx1.161，这是目前为止最新的稳定版本
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308104001981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjg0NjIx,size_16,color_FFFFFF,t_70)
- 安装之前其实网上也有一大堆的介绍安装nginx的教程，但都太过于繁琐了，不适合刚入门的小白来安装，下面就是我总结出的安装教程。
- 安装nginx之前先安装一些依赖

```
yum -y install gcc gcc-c++yum -y install gcc gcc-c++ autoconf automake make    //安装c编译器yum -y install pcre-devel openssl-devel
```

- wget下载nginx1.161版本，如我下载的路径为：**/usr/local/**

```
wget http://nginx.org/download/nginx-1.16.1.tar.gz
```

- 解压下载好的安装包

```
tar -zxvf nginx-1.16.1.tar.gz
```

- 进入到解压好的nginx目录下——》nginx-1.16.1，编译安装

```
./configure --prefix=/usr/local/nginxmake && make install
```

- 接着就会出现nginx的启动目录了
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308110819974.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjg0NjIx,size_16,color_FFFFFF,t_70)
- 进入到nginx目录下的，启动nginx，下面是常用的命令

```
./nginx            //启动./nginx -s stop    //停止./nginx -s reload    //重载配置
```

- 安装成功了：
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308111500493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjg0NjIx,size_16,color_FFFFFF,t_70)

**我这里给大家推荐两个快速启动nginx的方法，省的以后就不用每次都去 /usr/local/nginx/sbin 下执行命令了，就可以在任何目录下去执行nginx的命令了**

1. 建立个软连接，软连接类似于Windows下的快捷方式，**一般放在/usr/local/sbin或/usr/local/bin下去执行，sbin是root管理执行的目录，bin是普通用户管理执行的目录**。
   具体用法是：ln -s 源文件 目标文件。

```
ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin
```

进入到/usr/local/sbin目录下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308135656362.png)
如果不想用软连接，也可以删除掉。正确的删除方式（删除软链接，但不删除实际数据）

```
rm -rf 【软链接地址】
```

上述指令中，软链接地址最后不能含有“/”，当含有“/”时，删除的是软链接目标目录下的资源，而不是软链接本身。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308135846118.png)

1. 将nginx永久加入到系统环境变量，php、mysql 的方法跟这个配置一样，都是要配置XXX_HOME、PATH的。接下来配置环境变量在/etc/profile 中加入：

```
export NGINX_HOME=/usr/local/nginxexport PATH=$PATH:$NGINX_HOME/sbin
```

保存退出后，执行 **source /etc/profile** ，使配置文件生效。

接下来测试下：**nginx -v**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308141723677.png)
这个时候配置好了环境变量就可以在任何目录下执行nginx启动、停止或重装配置了，这个时候就不用 `./` 了

```
nginx            //启动nginx -s stop    //停止nginx -s reload    //重载配置
```

有关nginx具体介绍的安装步骤，可以访问：https://www.nginx.cn/install

标签：*Centos8.0编译安装稳定最新版的n**Nginx**Linux*

版权声明：本文为博主原创文章，转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1354639866087895041](https://www.kuangstudy.com/bbs/1354639866087895041)