# 安装docker

```
### 安装 Docker Engine-Community

### 使用 Docker 仓库进行安装

在新主机上首次安装 Docker Engine-Community 之前，需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker。

**设置仓库**

安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。

​```
sudo yum install -y yum-utils \
 device-mapper-persistent-data \
lvm2

使用以下命令来设置稳定的仓库。
​```
## 使用官方源地址（比较慢）

​```
sudo yum-config-manager \
  --add-repo \
  https://download.docker.com/linux/centos/docker-ce.repo

可以选择国内的一些源地址：
​```
## 阿里云

​```
sudo yum-config-manager \
  --add-repo \
  http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
​```
## 清华大学源
​```
sudo yum-config-manager \
  --add-repo \
  https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
​```
### 安装 Docker Engine-Community

安装最新版本的 Docker Engine-Community 和 containerd，或者转到下一步安装特定版本：

​```
 sudo yum install docker-ce docker-ce-cli containerd.io
​```

*如果提示您接受 GPG 密钥，请选是。*

> ***有多个 Docker 仓库吗？***
>
> *如果启用了多个 Docker 仓库，则在未在 yum install 或 yum update 命令中指定版本的情况下，进行的安装或更新将始终安装最高版本，这可能不适合您的稳定性需求。*

*Docker 安装完默认未启动。并且已经创建好 docker 用户组，但该用户组下没有用户。*

要安装特定版本的 Docker Engine-Community，请在存储库中列出可用版本，然后选择并安装：

1、列出并排序您存储库中可用的版本。此示例按版本号（从高到低）对结果进行排序。

​```
yum list docker-ce --showduplicates | sort -r
​```

*docker-ce.x86_64  3:18.09.1-3.el7           docker-ce-stable*
*docker-ce.x86_64  3:18.09.0-3.el7           docker-ce-stable*
*docker-ce.x86_64  18.06.1.ce-3.el7           docker-ce-stable*
*docker-ce.x86_64  18.06.0.ce-3.el7           docker-ce-stable*
```



# 启动docker

```
启动 Docker。

​```
 sudo systemctl start docker
​```

设置开机启动

​```
systemctl enable docker.service
​```


```

# 启动问题

```
问题描述**

安装完docker后，执行docker相关命令，出现：

​```
”Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.26/images/json: dial unix /var/run/docker.sock: connect: permission denied“
​```

**原因**

摘自docker mannual上的一段话：

​```
Manage Docker as a non-root user

The docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user root and other users can only access it using sudo. The docker daemon always runs as the root user.

If you don’t want to use sudo when you use the docker command, create a Unix group called docker and add users to it. When the docker daemon starts, it makes the ownership of the Unix socket read/writable by the docker group
​```

大概的意思就是：docker进程使用Unix Socket而不是TCP端口。而默认情况下，Unix socket属于root用户，需要root权限才能访问。

**解决方法1**

使用sudo获取管理员权限，运行docker命令

**解决方法2**

docker守护进程启动的时候，会默认赋予名字为docker的用户组读写Unix socket的权限，因此只要创建docker用户组，并将当前用户加入到docker用户组中，那么当前用户就有权限访问Unix socket了，进而也就可以执行docker相关命令

​```
sudo groupadd docker     #添加docker用户组
sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中
newgrp docker     #更新用户组
docker ps    #测试docker命令是否可以使用sudo正常使用
​```

# 
```



# 卸载docker

```
### 卸载旧版本

较旧的 Docker 版本称为 docker 或 docker-engine 。如果已安装这些程序，请卸载它们以及相关的依赖项。

​```
sudo yum remove docker \
         docker-client \
         docker-client-latest \
         docker-common \
         docker-latest \
         docker-latest-logrotate \
         docker-logrotate \
         docker-engine
​```

### 卸载docker

​```
yum remove docker-ce docker-ce-cli containerd.io
//卸载依赖
 rm -rf /var/lib/docker //删除资源
 rm -rf /var/lib/docker 默认工作路径
 rm -rf /etc/docker
​```


```

# 开发环境安装

## 1、java

##  

## 2、python

## 3、node

## 4、



# 容

# 器相关

```
-----------------------------
>>>>>>>>>>>>>>>>>>[停止一个容器] 
docker stop <容器 ID>
>>>>>>>>>>>>>>>>>>[停止的容器可以通过 docker restart 重启：] 
docker restart <容器 ID>
>>>>>>>>>>>>>>>>>>[进入容器]
在使用  -d 参数时，容器启动后会进入后台。此时想要进入容器，可以通过以下指令进入：
- docker attach
- docker exec：推荐大家使用 docker exec 命令，因为此退出容器终端，不会导致容器的停止。
>>>>>>>>>>>>>>>>>>[attach 命令]
下面演示了使用 docker attach 命令。
 docker attach 1e560fca3906 
 **注意：** 如果从这个容器退出，会导致容器的停止。
>>>>>>>>>>>>>>>>>>[exec 命令]
 下面演示了使用 docker exec 命令。
 docker exec -it 243c32535da7 /bin/bash
 **注意：** 如果从这个容器退出，容器不会停止，这就是为什么推荐大家使用 **docker exec** 的原因。
 >>>>>>>>>>>>>>>>>>[导出和导入容器]
**导出容器**
如果要导出本地某个容器，可以使用 **docker export** 命令。
docker export 1e560fca3906 > ubuntu.tar
导出容器 1e560fca3906 快照到本地文件 ubuntu.tar。
这样将导出容器快照到本地文件。
**导入容器快照**
可以使用 docker import 从容器快照文件中再导入为镜像，以下实例将快照文件 ubuntu.tar 导入到镜像 test/ubuntu:v1:
 cat docker/ubuntu.tar | docker import - test/ubuntu:v1
 此外，也可以通过指定 URL 或者某个目录来导入，例如：
 docker 
-----------------------------
 >>>>>>>>>>>>>>>>>>[删除容器使用 **docker rm** 命令：
docker rm -f 1e560fca3906
 >>>>>>>>>>>>>>>>>>[下面的命令可以清理掉所有处于终止状态的容器。
 docker container prune
>>>>>>>>>>>>>>>>>>[删除所有已停止的容器]
docker rm $(docker ps -a -q)
>>>>>>>>>>>>>>>>>>[删除所有镜像] 
docker rmi $(docker images -q)

>>>>>>>>>>>>>>>>>>[强制删除所有镜像]
docker rmi -f $(docker images -q)
import http://example.com/exampleimage.tgz example/imagerepo
 
```

