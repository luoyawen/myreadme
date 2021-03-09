## deepin安装docker 

```
1.//Docker要求系统内核版本高于3.10，查看Linux内核
uname -r

2.//使用以下命令更新apt包索引
sudo apt-get update

3.//安装以下包以允许apt通过HTTPS使用存储库
sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common

4.//添加Docker的官方GPG密钥并载入密钥，载入密钥后会提示OK
wget https://download.docker.com/linux/debian/gpg
sudo apt-key add gpg

5.//添加软件源
sudo vim /etc/apt/sources.list
deb https://download.docker.com/linux/debian stretch stable

6.//更新apt包索引
sudo apt-get update

7.//安装最新版本的Docker CE和containerd
sudo apt-get install docker-ce docker-ce-cli containerd.io

8.//使用docker version查看是否安装成功
```



如果报错为：

```text
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.38/containers/create: dial unix /var/run/docker.sock: connect: permission denied.
```

解决方案：运行以下两条命令：

```text
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# Docker启动时提示Get Permission Denied while trying to connect解决方法

## 环境描述

vmware15虚拟机安装centos7.4 64位系统，docker版本19.03.2

## 问题描述

安装完docker后，执行docker相关命令

```
docker run ubuntu:15.10 /bin/echo "Hello world"
```

出现如下提示：

> docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/create: dial unix /var/run/docker.sock: connect: permission denied.

原因
摘自docker mannual上的一段话

> Manage Docker as a non-root user
>
> The docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user root and other users can only access it using sudo. The docker daemon always runs as the root user.
>
> If you don’t want to use sudo when you use the docker command, create a Unix group called docker and add users to it. When the docker daemon starts, it makes the ownership of the Unix socket read/writable by the docker group.

大概的意思就是：docker进程使用Unix Socket而不是TCP端口。而默认情况下，Unix socket属于root用户，需要root权限才能访问。

### 解决方法1

使用sudo获取管理员权限，运行docker命令

```
#如下命令可以正常执行
sudo docker run ubuntu:15.10 /bin/echo "Hello world"
```

### 解决方法2

docker守护进程启动的时候，会默认赋予名字为docker的用户组读写Unix socket的权限，因此只要创建docker用户组，并将当前用户加入到docker用户组中，那么当前用户就有权限访问Unix socket了，进而也就可以执行docker相关命令

```
sudo groupadd docker     #添加docker用户组
sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中
newgrp docker     #更新用户组
docker ps    #测试docker命令是否可以使用sudo正常使用
```

本文转自：https://www.fengjunzi.com/blog-25467.html
