## Nginx常用命令

```http
cd /usr/local/nginx/sbin/
./nginx  启动
./nginx -s stop  停止
./nginx -s quit  安全退出
./nginx -s reload  重新加载配置文件
ps aux|grep nginx  查看nginx进程
```

启动成功访问 服务器ip:80

## 相关命令

```
1.	# 开启
2.	service firewalld start
3.	# 重启
4.	service firewalld restart
5.	# 关闭
6.	service firewalld stop
7.	# 查看防火墙规则
8.	firewall-cmd --list-all
9.	# 查询端口是否开放
10.	firewall-cmd --query-port=8080/tcp
11.	# 开放80端口
12.	firewall-cmd --permanent --add-port=80/tcp
13.	# 移除端口
14.	firewall-cmd --permanent --remove-port=8080/tcp
15.	
16.	#重启防火墙(修改配置后要重启防火墙)
17.	firewall-cmd --reload
18.	
19.	# 参数解释
20.	1、firwall-cmd：是Linux提供的操作firewall的一个工具；
21.	2、--permanent：表示设置为持久；
22.	3、--add-port：标识添加的端口；

```

## 演示

```
upstream luoyawen{//负载均衡 默认轮巡服务
 server 127.0.0.1:8989 weight=2;//weight权重
 server 127.0.0.1:8988 weight=1;
}

location / {//反向代理
  proxy_pass http://luoyawen;
}

```

## nginx绑定域名

```
server {
        listen       80; #首先是nginx的监听端口默认为80
        server_name  www.xxxx.com; #这里是你需要访问的域名地址
		#add_header 'Access-Control-Allow-Origin' '*';#这里是http 域名跨域，报错时候添加的请求头，这样写所有请求都会进来，会很不安全。
        #charset koi8-r;
        #access_log  logs/host.access.log  main;#这里是 日志文件的生成路径
        
		#详细介绍location
        location / {
        	#是监听的端口默认访问的地址，这里如果没有做tomcat的转发则会进入nginx的html目录下的index.html
            root   html;
            
            #这里是编写监听到的请求所转发的端口号，即tomcat端口
			proxy_pass http://localhost:8081;
            #Proxy Settings;
            #proxy_redirect off;
            #proxy_set_header Host $host;
            #proxy_set_header X-Real-IP $remote_addr;
            #proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			
			#设置nginx 的默认显示页
            index  index.html index.htm;

			#设置http请求的请求头，使其在跨域访问上不会被浏览器阻止。ps:这里设置我发现没有用，后来还是在ajax过滤器中添加的 请求头，如果大家有知道这里怎么修改的，请留言大家一起学习。
			add_header 'Access-Control-Allow-Origin' '*';
			add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
			add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```

