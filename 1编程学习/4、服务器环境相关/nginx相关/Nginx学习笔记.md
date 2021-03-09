# Nginx学习笔记

[![img](https://thirdwx.qlogo.cn/mmopen/vi_32/Q0j4TwGTfTK6l5KibDicShLPcsz7kbiaHRDoL4oMMp5LfO0j8eUvQbIa9dz3orYtFXnWvAVGqJW3J46SVIAGjianrA/132) 遇见你真好 ](https://www.kuangstudy.com/user/ac2d2b7a5dd94ce7a4b9ad3cef8503a2)分类：[学习笔记](https://www.kuangstudy.com/bbs?cid=4) 浏览：79 [评论：0](https://www.kuangstudy.com/bbs/1355002592891101185#comments)[收藏](javascript:void(0);)最后修改于： 2021/01/29 12:36:31

[展开目录+](javascript:void(0);)

## Nginx的简介

#### 1、什么是Nginx？

```
Nginx (engine x) 是一个轻量级、高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。在BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好。
```

#### 2、正向代理和反向代理的概述？

**正向代理**

```
正向代理（forward proxy）：是一个位于客户端和目标服务器之间的服务器(代理服务器)，为了从目标服务器取得内容，客户端向代理服务器发送一个请求并指定目标，然后代理服务器向目标服务器转交请求并将获得的内容返回给客户端。
```

![img](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2021/01/29/kuangstudyf012002b-6f28-4f2e-8a96-f5d8a60edeb2.jpg)

**反向代理**

```
以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。
```

![img](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2021/01/29/kuangstudyfd6419ca-989a-48ed-9d89-e8bb2cbda036.jpg)

```
举例说明：以租房子为例，就是我们以为我们接触的是房东，其实有时候也有可能并非房主本人，有可能是他的亲戚、朋友，甚至是二房东。但是我们并不知道和我们沟通的并不是真正的房东。这种帮助真正的房主租房的二房东其实就是反向代理服务器。这个过程就是反向代理。
```

#### 3、功能特点？

**支持操作系统**

```
FreeBSD 3— 10 / i386; FreeBSD 5— 10 / amd64;Linux 2.2— 4 / i386; Linux 2.6— 4 / amd64; Linux 3— 4 / armv6l, armv7l, aarch64;Solaris 9 / i386, sun4u; Solaris 10 / i386, amd64, sun4v;AIX 7.1 / powerpc;HP-UX 11.31 / ia64;Mac OS X / ppc, i386;Windows XP, Windows Server 2003,Windows 10
```

**结构与扩展**

```
一个主进程和多个工作进程。工作进程是单线程的，且不需要特殊授权即可运行；kqueue (FreeBSD 4.1+),epoll (Linux 2.6+),rt signals (Linux 2.2.19+),/dev/poll (Solaris 7 11/99+),select，以及 poll 支持；kqueue支持的不同功能包括 EV_CLEAR,EV_DISABLE （临时禁止事件）， NOTE_LOWAT,EV_EOF，有效数据的数目，错误代码；sendfile (FreeBSD 3.1+),sendfile (Linux 2.2+),sendfile64 (Linux 2.4.21+），和 sendfilev (Solaris 8 7/01+) 支持；输入过滤 (FreeBSD 4.1+) 以及 TCP_DEFER_ACCEPT (Linux 2.4+) 支持；10,000 非活动的 HTTP keep-alive 连接仅需要 2.5M内存。最小化的数据拷贝操作；其他HTTP功能；基于IP 和名称的虚拟主机服务；Memcached 的 GET 接口；支持 keep-alive 和管道连接；灵活简单的配置；重新配置和在线升级而无须中断客户的工作进程；可定制的访问日志，日志写入缓存，以及快捷的日志回卷；4xx-5xx错误代码重定向；基于 PCRE 的 rewrite 重写模块；基于客户端IP 地址和 HTTP 基本认证的访问控制；PUT,DELETE，和 MKCOL 方法；支持 FLV （Flash 视频）；带宽限制。实验特性内嵌的 perl；通过 aio_read()/aio_write() 的套接字工作的实验模块，仅在 FreeBSD 下；对线程的实验化支持，FreeBSD 4.x 的实现基于 rfork()；Nginx 主要的英语站点是 http://sysoev.ru/en/；英语文档草稿由 Aleksandar Lazic 完成。HTTP基础功能处理静态文件，索引文件以及自动索引；反向代理加速（无缓存），简单的负载均衡和容错；FastCGI，简单的负载均衡和容错；模块化的结构。过滤器包括gzipping,byte ranges,chunked responses，以及 SSI-filter。在SSI过滤器中，到同一个 proxy 或者 FastCGI 的多个子请求并发处理；SSL 和 TLS SNI 支持；IMAP/POP3代理服务功能：使用外部 HTTP 认证服务器重定向用户到 IMAP/POP3 后端；使用外部 HTTP 认证服务器认证用户后连接重定向到内部的 SMTP 后端；其他HTTP功能基于名称和基于IP的虚拟服务器；Keep-alive and pipelined connections support；保持活动和支持管线连接；Flexible configuration；灵活的配置；Reconfiguration and online upgrade without interruption of the client processing；重载配置，无间断程序升级；Access log formats,bufferred log writing,and quick log rotation；访问日志格式，bufferred日志写，快速登录旋转；3xx-5xx error codes redirection; 3xx的- 5xx错误代码重定向；The rewrite module；重写模块;Access control based on client IP address and HTTP Basic authentication；基于客户端IP地址访问控制和HTTP基本认证；The PUT,DELETE,MKCOL,COPY and MOVE methods; 提交，删除，MKCOL，复制和移动方法；FLV streaming;FLV视频流;Speed limitation；速度限制；Limitation of simultaneous connections or requests from one address.限制同个IP地址请求数量。Embedded perl.嵌入式的Perl。
```

**邮件代理服务器功能**

```
用户重定向到IMAP/POP3后端使用外部HTTP认证服务器；User authentication using an external HTTP authentication server and connection redirection to internal SMTP backend；用户身份验证使用外部HTTP认证服务器和连接重定向到内部的SMTP后端；Authentication methods：验证方法：POP3: USER/PASS,APOP,AUTH LOGIN/PLAIN/CRAM-MD5；的POP3：用户名/密码，的APOP，AUTH的LOGIN/PLAIN/CRAM-MD5;IMAP: LOGIN,AUTH LOGIN/PLAIN/CRAM-MD5; IMAP的：登录，AUTH的LOGIN/PLAIN/CRAM-MD5;SMTP: AUTH LOGIN/PLAIN/CRAM-MD5；的SMTP：AUTH的LOGIN/PLAIN/CRAM-MD5;SSL support; SSL支持；STARTTLS and STLS support. STARTTLS的和补充的支持。认证方法POP3: POP3 USER/PASS,APOP,AUTH LOGIN PLAIN CRAM-MD5;IMAP: IMAP LOGIN;SMTP: AUTH LOGIN PLAIN CRAM-MD5;SSL 支持；在 IMAP 和 POP3 模式下的 STARTTLS 和 STL支持。
```

#### 4、安装与配置

**Nginx安装**

**nginx + substitutions 安装（了解）**

```
nginx + substitutions 安装nginx 自带一个Substitution模块，但该模块只能写一行，所以我们改用 substitutions下面是安装一些预备软件（为编译安装做准备）yum -y --noplugins install wget zipyum -y --noplugins install unzipyum -y --noplugins install gccyum -y --noplugins install makeyum -y --noplugins install pcre-develyum -y --noplugins install openssl-devel
```

#### 第一步：软件包准备

```
nginx官网下载：http://nginx.org/en/download.html
```

#### 第二步：软件编译

```
tar zxf nginx-1.0.8.tar.gzcd nginx-1.0.8./configure ./configure --add-module=path/substitutions4nginx-read-only //注意这里的path是相对应的真实路径makemake install
```

**Nginx配置**

#### 配置文件内容

**全局块：配置服务器整体运行的配置指令**

```
eg. woker_process 1; //处理并发数的配置
```

**events块：影响Nginx服务器与用户的网络连接**

```
eg. woker_connections 1024; //支持的最大连接数
```

**http块**

```
包含 http全局块 、server块[server全局块、location块]########### 每个指令必须有分号结束。##################user administrator administrators;  #配置用户或者组，默认为nobody nobody。#worker_processes 2;  #允许生成的进程数，默认为1#pid /nginx/pid/nginx.pid;   #指定nginx进程运行文件存放地址error_log log/error.log debug;  #制定日志路径，级别。这个设置可以放入全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emergevents {    accept_mutex on;   #设置网路连接序列化，防止惊群现象发生，默认为on    multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off    #use epoll;      #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport    worker_connections  1024;    #最大连接数，默认为512}http {    include       mime.types;   #文件扩展名与文件类型映射表    default_type  application/octet-stream; #默认文件类型，默认为text/plain    #access_log off; #取消服务日志        log_format myFormat '$remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for'; #自定义格式    access_log log/access.log myFormat;  #combined为日志格式的默认值    sendfile on;   #允许sendfile方式传输文件，默认为off，可以在http块，server块，location块。    sendfile_max_chunk 100k;  #每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。    keepalive_timeout 65;  #连接超时时间，默认为75s，可以在http，server，location块。    upstream mysvr {         server 127.0.0.1:7878;      server 192.168.10.121:3333 backup;  #热备    }    error_page 404 https://www.baidu.com; #错误页    server {        keepalive_requests 120; #单连接请求上限次数。        listen       4545;   #监听端口        server_name  127.0.0.1;   #监听地址               location  ~*^.+$ {       #请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写。           #root path;  #根目录           #index vv.txt;  #设置默认页           proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表           deny 127.0.0.1;  #拒绝的ip           allow 172.18.5.54; #允许的ip                   }     }}
```

**上面是nginx的基本配置，需要注意的有以下几点：**

```
1、几个常见配置项：$remote_addr 与 $http_x_forwarded_for 用以记录客户端的ip地址；$remote_user ：用来记录客户端用户名称；$time_local ： 用来记录访问时间与时区；$request ： 用来记录请求的url与http协议；$status ： 用来记录请求状态；成功是200；$body_bytes_s ent ：记录发送给客户端文件主体内容大小；$http_referer ：用来记录从那个页面链接访问过来的；$http_user_agent ：记录客户端浏览器的相关信息；2、惊群现象：一个网路连接到来，多个睡眠的进程被同事叫醒，但只有一个进程能获得链接，这样会影响系统性能。3、每个指令必须有分号结束。
```

#### 5、配置实例

**反向代理实例**

**实例一**

```
server {        listen       80;        server_name  127.0.0.1;        #charset koi8-r;        #access_log  logs/host.access.log  main;        location / {            root   html;            proxy_pass http://127.0.0.1:8888;            index  login.html login.htm;        }    }
```

这里监听了本地的80端口，当访问localhost的时候会代理至localhost:8080

**实例二**

```
server {        listen       9001;        server_name  127.0.0.1;        #charset koi8-r;        #access_log  logs/host.access.log  main;        location / {            root   html;            proxy_pass http://127.0.0.1:8888;            index  login.html login.htm;        }        location ~/cboard {            proxy_pass http://127.0.0.1:8888;        }    }
```

这里监听了9001端口，当输入localhost的时候访问127.0.0.1:8888,当输入localhost的时候也是去访问localhost当时页面会带上上下文，此时访问的是本地开好的项目

**负载均衡实例**

```
upstream myserver{    
server localhost:8080;   
server localhost:8081;
}
server{  
listen       80;  
server_name  127.0.0.1;  
location / {         
root   html;        
proxy_pass http://myserver;     
index  login.html login.htm;     
}}
```

**nginx 分配服务器策略**

```
轮询（默认）每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器 down 掉，能自动剔除。weightweight 代表权重默认为 1,权重越高被分配的客户端越多ip_hash每个请求按访问 ip 的 hash 结果分配，这样每个访客固定访问一个后端服务器fair（第三方）按后端服务器的响应时间来分配请求，响应时间短的优先分配。
```

**nginx 常用命令**

```
//查看版本号nginx -v//启用nginx//快速停止nginxnginx -s stop//完整慢停止nginx -s quit//重新加载nginx -s reload//查看进程ps aux|grep nginx//检查配置文件nginx -t -c filepath
```

版权声明：本文为博主原创文章，转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1355002592891101185](https://www.kuangstudy.com/bbs/1355002592891101185)