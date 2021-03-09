## 【Linux 常用命令学习1

```
===================目录操作========================
mkdir: 创建目录
　　-p : 递归的创建目录 也就是可以创建多层目录
　　一次创建多个目录： mkdir {a,b,c,d,e,f}
　　一次创建 a b c d e f多个目录。
rmdir：删除一个空文件夹
cp：复制文件或者文件夹

　　-a =-pdr
　　-p 同时复制文件属性，比如修改日期
　　-d 复制时保留文件链接
　　-r: 复制文件夹时,递归复制子文件夹
　　-l 不复制，而是创建指向源文件的链接文件，链接文件名由目标文件给出。   
　　note:可以在拷贝的同时重命名
mv：移动文件或者文件夹，可以在移动的时候重命名

rm ：删除文件或者文件夹
　　-r：递归删除
　　-f：强制删除 即没有提醒

======================文件处理命令==============================
ls :查看文件
　　-l 以列表形式查看
　　-h 以一种人性化的方式查看，也是文件的大小以合适的单位显示
　　-a 查看所有文件，包括隐藏文件
　　-i 显示出文件的i节点号
touch 文件名：创建文件 可以一次创建多个文件，以空格隔开

cat :查看文件内容 
　　-n:带行号
tac:反向显示文件内容

more：分页查看文件内容
　　进入浏览模式后：
　　f或者空格：下一页
　　enter：一行一行往下翻
　　q:退出

less:查看文件内容 
　　空格翻页
　　回车换行
　　pageup：上一页
　　pagedown：下一页
　　上箭头：向上翻
　　下箭头：向下翻
　　/搜索词 n向下找

head -n 文件名 :查看文件前n行。缺省-n显示前10行
tail -n 文件名 ：查看文件的末尾几行
　　-f :动态显示文件末尾内容

ln:链接命令
　　-s创建软连接
　　硬链接和cp -p的区别是硬链接会同步更新
　　源文件如果丢失，硬链接依然存在。
　　硬链接和源文件的i节点相同。
　　硬链接不能夸分区，软连接可以跨分区。
　　硬链接不可以链接目录，链接可以
　　软连接文件具有的权限是ugo都是rwx

====================权限管理命令==============
chmod:修改文件或目录的权限，只有root和所有者可以更改
　　[{ugoa}{+-=}{rwx}] [文件或目录] 
　　[mode=421] [文件或目录]
　　-R 递归修改
　　权限的数字表示：
　　r->4
　　w->2
　　x->1

　　例：chmod u+x a.txt
　　　　chmod g+w,o-r a.txt //同时做多个权限的修改
　　　　chmod g=rwx a.txt
　　　　chmod 640 a.txt
　　　　chmod -R 777 testdir //把目录和下面所有文件的权限

　　　　　　　　　　　　针对文件　　 　　　　 针对目录
　　　　r　　 读权限 　　 可以查看文件内容　　  可以列出目录中的内容
　　　　w 　  写权限 　　 可以修改文件内容 　　 可以在目录中创建、删除文件
　　　　x 　   执行权限      可以执行文件 　　　　 可以进入目录

chown:更改文件所有者，只有root可以更改
　　chown root a.txt//把a.txt更改为root所有

chgrp:更改所属组
　　chgrp lambrother fengjie //把fengjie的所属组更改为lambrother

umask -S:查看创建文件的缺省权限，即默认权限
umask 023:修改文件的缺省权限为777-023=754。即rwxr-xr--

 

=====================文件搜索命令========================================
find:搜索制定范围内的文件
　　find [搜索范围] [匹配条件]
　　-name 按文件名搜索
　　-iname 根据文件名查找，不区分大小写
　　-size +n大于 -n小于 n等于 这个n是数据块，在Linux中一个数据块是512字节大小
　　-user 根据所有者查找
　　-group 根据所属组查找
　　根据文件属性查找：
　　　　-amin 访问时间 access
　　　　-cmin 根据文件属性被修改的时间 change
　　　　-mmin 根据文件内容被修改的时间 modify
　　例： find /etc -cmin -5 :查找/etc目录下五分钟内被修改过属性的文件和目录

　　-a 两个条件同时满足
　　　　find /etc -size +10 -a -size -50
　　-o 两个条件满足一个即可

　　-type 
　　　　f 文件 d 目录 l软连接文件
　　-inum 根据i节点查找

　　对找到的结果进行操作
　　　　-exec或者-ok 命令 {} \;
　　　　例如：
　　　　　　find /etc -name init* -exec ls -l {} \; 对找到的文件名按列表查看

　　find /etc -name init :搜索目录/etc下面所有的init文件，精确匹配，包括子目录中的init文件
　　find / -size +204800 搜索大于100M的文件

locate:(查找速度非常快，因为它维护了一个文件库。缺点就是新建立的文件没有很快收录到文件库)
　　locate 文件名
　　updatedb 更新locate的文件资料库 文件资料库不收录/tmp下的文件
　　-i 不区分大小写

which :查找命令的目录以及别名
　　which 命令

whereis :搜索命令所在目录及帮助文档路径。

grep:在文件中搜寻字符串匹配的行并输出，多个文件以空格隔开。
　　-i不区分大小写
　　-v排除指定字符串
　　-E 以正则表达式的方式搜索
　　-F 以普通文本的方式搜索
　　-n 显示搜索到的内容在文件中的行号。

==================帮助命令======================
man：查看命令或者配置文件的帮助信息
　　man 命令/配置文件
　　在手册里面，可以输入/要查找的str
　　man ls
　　man services
　　man fstab //直接输入配置文件的名字，而不需要使用绝对路径 重点查看name选项和配置文件的格式。
　　如果一个命令即使命令又是配置文件，那么可以使用一个序号进行区分，比如：
　　man 1 passwd 查看命令passwd的帮助
　　man 5 passwd 查看配置文件passwd的帮助

whatis 命令：得到命令的简要信息

apropos 配置文件名：查看配置文件的简短信息

命令 --help：查看命令的选项。

help 命令：查看shell内置命令的帮助信息。 shell内置命令是没有命令路径。不能使用man查看帮助。

===================用户管理命令==========================================
useradd: 添加用户
　　useradd 用户名

passwd: 修改用户密码
　　passwd 用户名 不加用户名直接更改自己的密码

who:查看当前的账户 显示的格式为： 登录用户名 登录终端（tty:本地登录 pts:远程终端） 登录时间 ip地址

　　w:查看更详细的用户登录信息。


=====================================压缩解压缩命令============================
.gz格式
　　压缩：gzip 文件名 只能压缩文件不能压缩目录，压缩完源文件也不见了
　　解压缩：gunzip/gzip -d 压缩包名称

tar:
　　-zcvf 压缩后文件名 打包的目录 :生成.tar.gz文件 注：这个命令先用tar归档，然后把归档的包压缩成.gz
　　-zxvf 要解压的文件名 ：解压缩.tar.bz2的文件

　　-jcvf 压缩后的文件名 打包的目录：生成.tar.bz2 注：这个命令先用tar归档，然后把归档的包压缩成.bz2
　　-jxvf 要解压的文件名 :解压.tar.bz2的文件

zip:
　　zip -r 压缩生成的文件名 要压缩的目录
　　zip 压缩生成的文件名 要压缩的文件。

unzip:
　　unzip 要解压缩的文件

bzip2:
　　bzip2 -k 要压缩的文件名 -k选项：保留源文件
　　bunzip2 -k 要解压的文件名 -k选项：保留压缩包

 

===============网络命令==========================
write:给在线用户发送信息，用户不在线不行。以Ctrl+D保存
　　write 用户名

wall:给所有用户名发送信息
　　wall 要发送的信息

ping:测试网络连通性

　　ping ip地址 
　　-c 要ping的次数

ifconfig:
　　直接回车查看当前网卡信息
　　ifconfig 网卡名 ip地址 临时修改网络ip
　　　　ifconfig th0:0 192.168.1.100 netmask 255.255.255.0
　　　　　　给th0这个网卡新添加一个ip
　　　　ifconfig eth0:0 down
　　　　ifconfig eth0:0 up
ifdown th0
　　禁用th0这块网卡

ifup th0
　　开启th0这块网卡

mail:邮件命令
　　mail 要发送的用户名
　　mail 直接回车：查看命令
　　　　help :查看支持的命令格式
　　　　输入序列号：查看邮件详细内容
　　　　h: 回到邮件列表
　　　　d 序列号：删除序列号对应的邮件

last:统计计算机所有用户登录的时间信息，以及重启信息
lastlog:所有用户最后一次登录的时间
　　-u 用户的uid 查看指定用户的登录信息。

traceroute:显示数据包到主机间的路径
　　traceroute 要探测的地址.
　　-n 使用ip而不使用域名

nslookup www.baidu.com
　　查看百度的ip地址

netstat:显示网络相关信息
　　-t :tcp协议
　　-u :udp协议
　　-l:监听
　　-r:路由
　　-n:显示ip地址和端口号

　　netstat -tlun:查看本机监听的端口
　　netstat -an:查看所有的监听信息
　　netstat -rn ：查看路由表，即网管

wget 文件地址
　　下载文件

service network restart:重启网络服务。

telnet 域名或ip
　　远程管理与端口探测
　　如： telnet 192.168.2.3:80
　　　　探测192.168.2.3是否开启了80端口

mount:挂在命令
　　mount -t iso9660 /dev/sr0 /mnt/cdrom :把sr0挂在到cdrom

==============关机重启命令====================

shutdown:这个关机命令更安全一些，不推荐使用其他关机命令。
　　-h：关机
shutdown -h now shutdown -h 20:30
　　-r:重启 
shutdown -r now 
　　-c:取消上次的关机命令

重启：
　　init 6
　　reboot

关机：
　　init 0
　　poweroff

　　系统运行级别：
　　　　0 关机
　　　　1 单用户 类似windows安全模式
　　　　2 不完全多用户，不含nfs服务
　　　　3 完全多用户
　　　　4 未分配
　　　　5 图形界面
　　　　6 重启
　　可以通过查看/etc/inittab来查看系统启动的运行级别
　　runlevel:查看当前的运行级别
　　init n:设置系统运行级别

logout:退出当前用户，返回到登录界面

 

==============其他小技巧==========
\命令名字 :使用原始的命令
　　比如：
　　　　ls 实际上是ls --color auto
　　　　\ls 就是原始的ls


=============================================
一、软件包分类
　　源码包
　　　　脚本安装包
　　特点：
　　　　1. 开源
　　　　2. 可以自由选择所需的功能
　　　　3. 软件是编译安装，所以更加适合自己的系统，更加稳定也效率更高
　　　　4. 卸载方便，即可以直接删除文件夹。
　　缺点：
　　　　1. 安装过程步骤较多，尤其安装较大的软件集合时，容易出现错误
　　　　2. 编译时间较长，安装毕二进制安装时间长
　　　　3. 因为是编译安装，安装过程中一旦报错新手很难解决


　　二进制包(RPM包、系统默认包)
　　　　优点：
　　　　　　1. 包管理系统简单，只通过几个命令就可以实现包的安装、升级、查询和卸载
　　　　　　2. 安装速度比源码包安装快的多
　　　　缺点：
　　　　　　1. 经过编译，不再可以看到源代码
　　　　　　2. 功能选择不如源码包灵活
　　　　　　3. 依赖性

=============rpm命令管理-包命名与依赖性=======================================
1. RPM包命名原则
　　httpd-2.2.15-15.el6.centos.l.i686.rpm
　　　　httpd 软件包名
　　　　2.2.15 软件版本
　　　　15 软件发布的次数
　　　　el6.centos 适合的Linux平台
　　　　i686 适合的硬件平台
　　　　rpm rpm包扩展名
　　　　如果名字里有noarch,则表示所有平台都可以。

2、 rpm包依赖性
　　　　树形依赖： a->b->c 从后往前安装所依赖的包。
　　　　环形依赖： a->b->c->a 解决办法：一次性安装三个包
　　　　模块依赖：模块依赖查询网站 ：www.rpmfind.net 一般以.so.数字结尾的依赖包，是库依赖包，只需要安装包括这个库的软件就可以自动安装好这个所需的库依赖包

包全名：操作的包是没有安装的软件包时，使用包全名，而且要注意路径。安装、升级时用
包名 ：操作已经安装的软件包时，使用包名。是搜索/var/lib/rpm中的数据库。一般查询，卸载时用

3. rpm安装：
　　rpm-ivh 包全名
　　　　-i(install) 安装
　　　　-v(verbose) 显示详细信息
　　　　-h(hash) 显示进度
　　　　--nodeps 不检测依赖性 一般都必须要检测

4. rpm包升级：
　　rpm -Uvh 包全名
　　　　-U(upgrade) 升级
　　　　-h

5. rpm -e 包名
　　-e(erase) 卸载
　　--nodeps 不检查依赖性

6. 查询是否安装
　　rpm - q 包名 :查询包是否安装
　　　　-q(query) 查询
　　　　-a(all) 所有
　　　　-i(information) 查询软件信息
　　　　-p(package) 查询未安装包信息
　　rpm -ql 包名：查询包中文件安装位置(list) 注：包的安装路径在包生成的时候就确定了
　　rpm -qlp 包全名：查询未安装包安装时会安装在哪里。
　　rpm -qf 系统文件名 ：查询系统文件属于哪个rpm包 注：系统文件名必须是通过安装哪个包生成的文件
　　　　-f:查询系统文件属于哪个包
　　rpm -qR 包名 查询已安装软件包的依赖性
　　　　-r: 查询软件包的依赖性(requires)
　　rpm -qRp:查询未安装包的依赖性
　　　　-p: 查询未安装包的依赖性

　　　　例如：
　　　　　　rpm -qa | grep httpd 查询所有Apache的包

7. rpm包校验
　　rpm -V 已安装的包名 ：如果没有提示则表示没有被修改过
　　　　-V 校验指定rpm包中的文件(verify)
　　　　校验值的含义：
　　　　　　S:文件大小是否改变
　　　　　　M:文件的类型或文件的权限(rwx)是否被改变
　　　　　　5：文件MD5校验和是否改变(可以看成文件内容是否改变)
　　　　　　D:设备的中，从代码是否改变
　　　　　　L:文件路径是否改变
　　　　　　U:文件的属主(所有者)是否改变
　　　　　　G:文件的属组是否改变
　　　　　　T:文件的修改时间是否改变

8. rpm包中文件提取：
　　rpm2cpio 包全名 | \
　　cpio -div .文件绝对路径

　　rpm2cpio:讲rpm包转换为cpio格式的命令 
　　\表示命令没有输完,在下一行继续输入
　　cpio:是一个标准工具，它用于创建软件档案文件和从档案文件中提取文件
　　cpio 选项 <[文件|设备]
　　　　-i copy-in模式，还原
　　　　-d:还原时自动新建目录
　　　　-v:显示还原过程

　　例如：
　　　　rpm -qf /bin/ls #查看ls命令属于哪个包
　　　　mv /bin/ls /tmp #将ls命令移走
　　　　rpm2cpio /mnt/cdrom/Packages/coreutils-8.4-19.el6.i686.rpm | cpio -idv ./bin/ls #提取rpm保重ls命令到当前目录的/bin/ls下
　　　　cp /root/bin/ls /bin/ #把ls命令复制到/bin/目录，修复文件丢失

 

yum在线管理：
一、 ip地址配置
第1步：setup:使用图形界面配置ip地址
第2步：vi/etc/sysconfig/network-scripts/ifcfg-eth0 把ONBOOT="no"改为ONBOOT="yes" #启动网卡
第3步：service network restart :重新启动网络服务。

二、网络yum源
1. yum源位置：/etc/yum.repos.d/CentOS-Base.repo,这个是默认的网络yum源
　　[base]    容器名称，一定要放在[]中
　　name  容器说明，可以自己随便写
　　mirrorlist    镜像站点，这个可以注释掉
　　baseurl   我们的yum源服务器的地址，默认是CentOS官方的yum源服务器，是可以使用的，如果你觉得慢可以改成你喜欢的yum源地址
　　enabled   此容器是否生效，如果不写或写成enable=1都是生效，写成enable=0就是不生效
　　gpgcheck  如果是1是指rpm的数字证书生效，如果是0则不生效
　　gpgkey    数字证书的公钥文件保存位置。不用修改。

2. yum命令
　　yum list :获取服务器上所有可用的软件的列表
　　yum search 关键字：搜索服务器上所有和关键字相关的包
　　yum -y install 包名：安装软件包
　　　　install:安装
　　　　-y:自动回答yes
　　yum -y update 包名：升级软件包
　　　　update:升级
　　　　-y:自动回答yes
　　　　如果没有包名，就会升级所有的软件包，包括Linux内核。慎用
　　yum -y remove 包名
　　　　remove:卸载
　　　　-y:自动回答yes
　　　　注：yum会自动卸载依赖包，而很有可能这个依赖包也被别的包依赖，所以很危险，慎用。

　　yum grouplist:列出所有可用的软件组列表
　　yum groupinstall 软件组名：安装指定软件组，组名可以由grouplist查询出来 注：如果查询出来的软件组名中间有空格，要使用""引起来。
　　yum groupremove 软件组名：卸载指定软件组

3. 光盘yum源
　　1) 挂在光盘 mount /dev/sr0 /mnt/cdrom 
　　2) 让网络yum源文件失效
　　　　cd /etc/yum.repos.d/
　　　　mv CentOS-Base.repo CentOS-Base.repo.bak
　　　　mv CentOS-Debuginfo.repo CentOS-Debuginfo.repo.bak
　　　　mv Centos-Vault.repo Centos-Vault.repo.bak
　　3) 修改光盘yum源文件
　　　　vim CentOS-Media.repo
　　　　[c6-media]
　　　　name=CentOS-$releaserver -Media
　　　　baseurl=file:///mnt/cdrom 
　　　　#地址为你自己的光盘挂载地址
　　　　#   file:///media/cdrom/
　　　　#   file:///media/cdrecorder/
　　　　#注释这两个不存在的地址
　　　　gpgcheck=1
　　　　enabled=1 #把enabled=0改为enabled=1，让这个yum配置文件生效
　　　　gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

　　　　注意：注释配置行的时候，#符号一定要写在开头，不要随便在配置文件某一行后面加注释，也不要随便加空格。

源码包管理
　　1. 区别
　　　　安装之前的区别：概念上的区别
　　　　安装之后的区别：安装位置不同

　　2. rpm包安装位置(大多数)
　　　　/etc/   配置文件安装目录
　　　　/usr/bin/   可执行的命令安装目录
　　　　/usr/lib/   程序所使用的函数库保存位置
　　　　/usr/share/doc  软件的基本使用书册保存位置
　　　　/usr/share/man/ 帮助文件保存位置    
　　3. 源码包安装位置
　　　　安装在指定位置当中，一般是
　　　　/usr/local/软件名/ 
　　4. 安装位置不同带来的影响
　　　　rpm包安装的服务可以使用系统服务管理命令(service)来管理
　　　　例如rpm包安装的Apache的启动方法是：
　　　　/etc/rc.d/init.d/httpd start 注：服务的安装路径一般在：/etc/rc.d/init.d下
　　　　service httpd start 注：service命令是红帽的专用命令,只能管理rpm包安装的服务
源码包安装过程
　　1. 安装准备
　　　　安装C语言编译器 gcc
　　　　下载源码包
　　　　http://mirror.bit.edu.cn/apach/httpd/   
　　2. 安装注意事项
　　　　源代码保存位置：/usr/local/src/
　　　　软件安装位置： /usr/local/
　　　　如何确定安装过程报错：
　　　　　　安装过程停止并出现error、warning或no的提示  
　　3. 源码包安装过程
　　　　1)下载源码包
　　　　2)解压缩下载的源码包
　　　　3)进入解压缩目录 注：里面有个INSTALL是系统安装的步骤说明
　　　　4)./configure 软件配置与检查
　　　　　　定义需要的功能选项
　　　　　　检测系统环境是否符合安装要求
　　　　　　把定义好的功能选项和检测系统环境的信息都写入Makefile文件，用于后续的编辑。
　　　　./configure --prefix=/usr/local/apache2 ：定义安装位置 
　　　　5)make :编译
　　　　　　如果前面有错误，则使用make clean命令清楚编译产生的临时文件
　　　　6)make install:编译安装
　　4. 源码包的卸载
　　　　不需要卸载命令，直接删除安装目录即可。不会遗留任何垃圾文件

脚本安装
　　1. 脚本安装包
　　　　脚本安装包并不是独立的软件包类型，常见安装的是源码包
　　　　是人为把安装过程写成了自动安装的脚本，只要执行脚本，定义简单的参数，就可以完成安装
　　　　非常类似于windows下软件的安装方式
　　2. Webmin的作用
　　　　Webmin是一个基于web的Linux系统管理界面，你就可以通过图形化的方式
　　　　设置用户账号、Apache，DNS、文件共享等服务。
　　3、 webmin安装过程
　　　　1) 下载软件
　　　　　　http;//sourceforge.net/projects/webadmin/files/webmin/
　　　　2) 解压缩，并进入解压缩目录
　　　　3) 执行安装脚本 ./setup.sh

 

其他命令

du -sh 文件名

ps 静态查看系统进程，系统默认安装
　　ps -aux 使用BSD语法查看所有进程
　　ps -ef 标准语法查看所有进程
　　　　UID 程序被该 UID 所拥有
　　　　PID 就是这个程序的 ID 
　　　　PPID 则是其上级父程序的ID
　　　　C CPU 使用的资源百分比
　　　　STIME 系统启动时间
　　　　TTY 登入者的终端机位置
　　　　TIME 使用掉的 CPU 时间。
　　　　CMD 所下达的指令为何
　　ps -aux --sort -pcpu,-pmem
　　　　根据CPU占用情况和内存占用情况来显示进程
　　watch -n 1 'ps -aux --sort -pcpu,-pmem'
　　　　每隔1秒监控一次进程情况

top 动态查看系统的状态

lsof -Pti :8000
　　通过端口号获得进程pid

kill -9 pid
　　杀死指定pid的进程，强行杀死。

history
　　查看历史命令

执行历史命令
　　!! 执行上一条命令
　　!n 执行历史命令的中第n条
　　!-n 执行导数第n条
　　!string 执行以string开头的历史命令行
　　!?string? 执行包含string的历史命令行


alias 
　　给命令起别名

　　alias 命令='别名'
　　alias -p 查看已存在的别名

unlias 
　　取消别名
　　unlias name

cal 
　　查看某一年的日历，可以是1-9999中的任意一年
　　cal 88

zcat
　　查看压缩包中的内容

sed -i 's#old#new#g' 文件名
　　使用new替换文件中的old

ssh root@192.168.8.15 "ifconfig"
　　远程执行命令

bash -x 脚本名
　　调试脚本

centos6上的三个网络配置文件
　　/etc/sysconfig/network-scripts/ifcfg-etho
　　/etc/sysconfig/network
　　/etc/resolv.conf # dns
```



> 

# 【linux下的打包与压缩

#### linux下的打包与压缩

```
zip -r -9 -q -l -o shiyanlou.zip /home/shiyanlou/Desktop//将目录 /home/shiyanlou/Desktop 打包成一个文件 /*-r 参数表示递归打包包含子目录的全部内容,  1 表示最快压缩但体积大，9表示体积最小但耗时最久,  -q 参数表示为安静模式，即不向屏幕输出信息,  -l   因为 Windows 系统与 Linux/Unix在文本文件格式上的一些兼容问题，比如换行符（为不可见字符），在 Windows为CR+LF（Carriage-Return+Line-Feed：回车加换行），而在Linux/Unix上为LF（换行），所以如果在不加处理的情况下，在 Linux 上编辑的文本，在 Windows 系统上打开可能看起来是没有换行的。 -l 参数将 LF 转换为 CR+LF 来达到以上目的  -o，表示输出文件*/
```

------

```
unzip shiyanlou.zip   //解压到当前文件夹unzip -l shiyanlou.zip     //查看不解压unzip -q shiyanlou.zip -d zipfolder   //使用安静模式，将文件解压到指定目录,目录不存在自动创建
unzip -O GBK 中文文件名.zip//使用 -O（英文字母，大写 o）参数指定编码类型：  通常 Windows 系统上面创建的压缩文件，如果有有包含中文的文档或以中文作为文件名的文件时默认会采用 GBK 或其它编码，而 Linux 上面默认使用的是 UTF-8 编码，如果不加任何处理，直接解压的话可能会出现中文乱码的问题（有时候它会自动帮你处理），为了解决这个问题，我们可以在解压时指定编码类型。
```

------

```
du -h shiyanlou.zip  // du 命令查看文件的大小
```

------

## cd 到所在文件夹打包则无父目录，在相对路径下打包减少父目录

[链接](https://blog.csdn.net/qiulinsama/article/details/86498686)

```
tar -v -P -xf shiyanlou.tar -C ~/tardir/**-v 可视化  -P 保留绝对路径，解压时再用-P会从根目录开始解压会原来的地方  -xf f必须紧跟文件名，x为解包  -C 目标路径**/
```

------

```
tar -P -cf shiyanlou.tar /home/shiyanlou/Desktop //创建tar ,其他同上，  //-cf 创建一个tar文件, f后紧跟文件名
tar -tf shiyanlou.tar//只查看不解包
```

版权声明：本文为博主原创文章，转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1354997873707589634](https://www.kuangstudy.com/bbs/1354997873707589634)

# 【Linux常用基本命令——目录管理

[![img](https://thirdwx.qlogo.cn/mmopen/vi_32/DYAIOgq83epwskB5DTvfu2ZU7qjicES2ibPwdHMY4owmemSJ1DF9xtTGjDvm6QXI94AicJhMbxK7SILB4micIibHg3A/132) 动 ](https://www.kuangstudy.com/user/312c6e3582ec4a74a8dc0dd6db6ed11e) 分类：[学习笔记](https://www.kuangstudy.com/bbs?cid=4) 浏览：83 [评论：0](https://www.kuangstudy.com/bbs/1355054016664559617#comments)[收藏](javascript:void(0);)最后修改于： 2021/01/29 15:23:48

[展开目录+](javascript:void(0);)

## 绝对路径和相对路径

道Linux的目录结构为树状结构，最顶级的目录为根目录 /。

其他目录通过挂载可以将它们添加到树中，通过解除挂载可以移除它们。

在开始本教程前我们需要先知道什么是**绝对路径**与**相对路径**。

### **绝对路径：**

路径的写法，由根目录 / 写起，例如： /usr/share/doc 这个目录。

### **相对路径：**

路径的写法，不是由 / 写起，例如由 /usr/share/doc 要到 /usr/share/man 底下时，可以写成： cd../man 这就是相对路径的写法啦！

## 处理目录的常用命令

接下来我们就来看几个常见的处理目录的命令吧：

- ls: 列出目录
- cd：切换目录
- pwd：显示目前的目录
- mkdir：创建一个新的目录
- rmdir：删除一个空的目录
- cp: 复制文件或目录
- rm: 移除文件或目录
- mv: 移动文件与目录，或修改文件与目录的名称

你可以使用 man [命令] 来查看各个命令的使用文档，如 ：man cp。

### ls （列出目录）

在Linux系统当中， ls 命令可能是最常被运行的。
语法：

```
[root@www ~]# ls [-aAdfFhilnrRSt] 目录名称
```

选项与参数：

```
-a ：全部的文件，连同隐藏文件( 开头为 . 的文件) 一起列出来(常用)-l ：长数据串列出，包含文件的属性与权限等等数据；(常用)
```

将目录下的所有文件列出来(含属性与隐藏档)

```
[root@www ~]# ls -al ~
```

### cd （切换目录）

cd是Change Directory的缩写，这是用来变换工作目录的命令。
语法：

```
cd [相对路径或绝对路径]
```

测试：

```
# 切换到用户目录下 [root@kuangshen /]# cd home # 使用 mkdir 命令创建 kuangstudy 目录 [root@kuangshen home]# mkdir kuangstudy # 进入 kuangstudy 目录 [root@kuangshen home]# cd kuangstudy # 回到上一级 [root@kuangshen kuangstudy]# cd .. # 回到根目录 [root@kuangshen kuangstudy]# cd / # 表示回到自己的家目录，亦即是 /root 这个目录 [root@kuangshen kuangstudy]# cd ~
```

接下来大家多操作几次应该就可以很好的理解 cd 命令的。

### pwd ( 显示目前所在的目录 )

pwd 是 **Print Working Directory** 的缩写，也就是显示目前所在目录的命令。

```
[root@kuangshen kuangstudy]#pwd [-P]
```

选项与参数： **-P** ：显示出确实的路径，而非使用连结 (link) 路径。
测试：

```
# 单纯显示出目前的工作目录 [root@kuangshen ~]# pwd /root # 如果是链接，要显示真实地址，可以使用 -P参数 [root@kuangshen /]# cd bin [root@kuangshen bin]# pwd -P /usr/bin
```

### mkdir （创建新目录）

如果想要创建新的目录的话，那么就使用mkdir (make directory)吧。

```
mkdir [-mp] 目录名称
```

选项与参数：

```
-m ：配置文件的权限喔！直接配置，不需要看默认权限 (umask) 的脸色～-p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来！
```

测试：

```
# 进入我们用户目录下 [root@kuangshen /]# cd /home # 创建一个 test 文件夹 [root@kuangshen home]# mkdir test # 创建多层级目录 [root@kuangshen home]# mkdir test1/test2/test3/test4 mkdir: cannot create directory ‘test1/test2/test3/test4’: No such file or directory # <== 没办法直接创建此目录啊！ # 加了这个 -p 的选项，可以自行帮你创建多层目录！ [root@kuangshen home]# mkdir -p test1/test2/test3/test4 # 创建权限为 rwx--x--x 的目录。 [root@kuangshen home]# mkdir -m 711 test2 [root@kuangshen home]# ls -l drwxr-xr-x 2 root root 4096 Mar 12 21:55 test drwxr-xr-x 3 root root 4096 Mar 12 21:56 test1 drwx--x--x 2 root root 4096 Mar 12 21:58 test2
```

### rmdir ( 删除空的目录 )

语法：

```
rmdir [-p] 目录名称
```

选项与参数：**-p** ：连同上一级『空的』目录也一起删除
测试：

```
# 看看有多少目录存在？ [root@kuangshen home]# ls -l drwxr-xr-x 2 root root 4096 Mar 12 21:55 test drwxr-xr-x 3 root root 4096 Mar 12 21:56 test1 drwx--x--x 2 root root 4096 Mar 12 21:58 test2 # 可直接删除掉，没问题 [root@kuangshen home]# rmdir test # 因为尚有内容，所以无法删除！ [root@kuangshen home]# rmdir test1 rmdir: failed to remove ‘test1’: Directory not empty # 利用 -p 这个选项，立刻就可以将 test1/test2/test3/test4 依次删除。 [root@kuangshen home]# rmdir -p test1/test2/test3/test4
```

注意：这个 **rmdir** 仅能删除**空的**目录，你可以使用 **rm** 命令来删除**非空**目录。

### cp ( 复制文件或目录 )

语法：

```
[root@www ~]# cp [-adfilprsu] 来源档(source) 目标档(destination) [root@www ~]# cp [options] source1 source2 source3 .... directory
```

选项与参数：

- -a：相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)
- -p：连同文件的属性一起复制过去，而非使用默认属性(备份常用)；
- -d：若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；
- -r：递归持续复制，用於目录的复制行为；(常用)
- -f：为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；
- -i：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
- -l：进行硬式连结(hard link)的连结档创建，而非复制文件本身。
- -s：复制成为符号连结档 (symbolic link)，亦即『捷径』文件；
- -u：若 destination 比 source 旧才升级 destination ！

测试：

```
# 找一个有文件的目录，我这里找到 root目录 [root@kuangshen home]# cd /root [root@kuangshen ~]# ls install.sh[root@kuangshen ~]# cd /home# 复制 root目录下的install.sh 到 home目录下 [root@kuangshen home]# cp /root/install.sh /home [root@kuangshen home]# ls install.sh # 再次复制，加上-i参数，增加覆盖询问？ [root@kuangshen home]# cp -i /root/install.sh /home cp: overwrite ‘/home/install.sh’? y # n不覆盖，y为覆盖
```

### rm ( 移除文件或目录 )

语法：

```
rm [-fir] 文件或目录
```

选项与参数：

- -f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；
- -i ：互动模式，在删除前会询问使用者是否动作
- -r ：递归删除啊！最常用在目录的删除了！这是**非常危险**的选项！！！

测试：

```
# 将刚刚在 cp 的实例中创建的 install.sh删除掉！ [root@kuangshen home]# rm -i install.sh rm: remove regular file ‘install.sh’? y # 如果加上 -i 的选项就会主动询问喔，避免你删除到错误的档名！ # 尽量不要在服务器上使用 rm -rf /# 因为这只是删库，而你还没学会跑路
```

### mv ( 移动文件与目录，或修改名称 )

语法：

```
[root@www ~]# mv [-fiu] source destination [root@www ~]# mv [options] source1 source2 source3 .... directory 12
```

选项与参数：

- -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
- -i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
- -u ：若目标文件已经存在，且 source 比较新，才会升级 (update)

测试：

```
# 复制一个文件到当前目录 [root@kuangshen home]# cp /root/install.sh /home # 创建一个文件夹 test [root@kuangshen home]# mkdir test# 将复制过来的文件移动到我们创建的目录，并查看 [root@kuangshen home]# mv install.sh test [root@kuangshen home]# ls test [root@kuangshen home]# cd test [root@kuangshen test]# ls install.sh # 将文件夹重命名，然后再次查看！ [root@kuangshen test]# cd .. [root@kuangshen home]# mv test mvtest [root@kuangshen home]# ls mvtest
```

标签：*Linux*

版权声明：本文为博主原创文章，转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1355054016664559617](https://www.kuangstudy.com/bbs/1355054016664559617)

# 【shell系列——文件基本操作

[![img](https://thirdwx.qlogo.cn/mmopen/vi_32/DYAIOgq83ers6MQH8vPG744M3uFmV3Ak7fTFzlGU7sssDZaibqiaqXUuhnWKKtFy3YTXpHUpS9yicuNcM3ic0JhTiag/132) ? ? ? ? ](https://www.kuangstudy.com/user/dfa189b5e5784360bb083cce44015102)分类：[运维](https://www.kuangstudy.com/bbs?cid=9) 浏览：39 [评论：0](https://www.kuangstudy.com/bbs/1353708733523394562#comments)[收藏](javascript:void(0);)最后修改于： 2021/01/25 22:18:49

[展开目录+](javascript:void(0);)

## 常见Linux目录名称

| 目录   | 说明                                                      |
| ------ | --------------------------------------------------------- |
| /      | 虚拟目录的根目录。通常不会在这里存储文件                  |
| /bin   | 二进制目录，存放许多用户级的GNU工具                       |
| /boot  | 启动目录，存放启动文件                                    |
| /dev   | 设备目录，Linux在这里创建设备节点                         |
| /etc   | 系统配置文件目录                                          |
| /home  | 主目录，Linux在这里创建用户目录                           |
| /lib   | 库目录，存放系统和应用程序的库文件                        |
| /media | 媒体目录，可移动媒体设备的常用挂载点                      |
| /mnt   | 挂载目录，另一个可移动媒体设备的常用挂载点                |
| /opt   | 可选目录，常用于存放第三方软件包和数据文件                |
| /proc  | 进程目录，存放现有硬件及当前进程的相关信息                |
| /root  | root用户的主目录                                          |
| /sbin  | 系统二进制目录，存放许多GNU管理员级工具                   |
| /run   | 运行目录，存放系统运作时的运行时数据                      |
| /srv   | 服务目录，存放本地服务的相关文件                          |
| /sys   | 系统目录，存放系统硬件信息的相关文件                      |
| /tmp   | 临时目录，可以在该目录中创建和删除临时工作文件            |
| /usr   | 用户二进制目录，大量用户级的GNU工具和数据文件都存储在这里 |
| /var   | 可变目录，用以存放经常变化的文件，比如日志文件            |

## 查看文件列表

```
ls -XX [option]
ls 显示当前目录文件   
-F    区分文件夹和文件   
-a    显示隐藏文件    
-l    详细信息   
-R    级联展开文件夹 
-i    查看文件或目录的inode编号  
option   过滤器 ?单字符  * 一个或多个  
[]限定例子：ls -Fl my*       
显示my开头的文件列表
```

## 创建、复制、重命名、移动、删除

```
//创建空文件touch filename
//创建目录mkdir New_Dir
//创建目录及子目录 mkdir -p New_Dir/Sub_Dir/Under_Dir
//删除空目录rmdir New_Dir
//复制目录或文件cp -XX oldfile newfilels 显示当前目录文件    -i    询问是否覆盖同名newfile文件    -R    递归复制，文件需要加后缀/
//重命名    mv oldfilename new filename
//移动,覆盖前询问mv -i oldfilename 文件夹/mv -i oldfilename 文件夹/newfilename
//删除rm -i fall
//级联删除并询问rm -ri My_Dir
//级联删除不询问rm -rf My_Dir
```

## 链接文件 （原始文件必须事先存在）

```
//符号链接ln -s filename  filename_副本//硬链接ln filename  filename_副本
```

## 查看整个文件

```
//查看文本文件cat -XX file    -n    显示行号包括空格    -b    显示行号跳过空格    -T    用^I 字符组合去替换文中的所有制表符//分页查看文件内容less -XX file//查看文件末尾10行tail file//查看文件开头10行head file//实时显示文件末尾100行tail -nf 100 filetail -100f file
```

标签：*shell**文件*