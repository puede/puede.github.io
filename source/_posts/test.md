---
title: Ubuntu Server Rsync服务端与Windows cwRsync客户端实现数据同步配置
date: 2018-01-27 14:00:00
tags:
---
Rsync(remote synchronize)是一个远程数据同步工具，可以使用“Rsync算法”同步本地和远程主机之间的文件。rsync的好处是只同步两个文件不同的部分，相同的部分不在传递。类似于增量备份，这使的在服务器传递备份文件或者同步文件，比起scp工具要省好多时间
<!-- more-->
### 一. 需要的工具

下载cwRsyncServer服务器端与客户端的安装文件：

服务端下载：[cwRsyncServer_4.0.5_Installer.zip](https://pan.baidu.com/share/link?shareid=901516432&uk=118355419 "服务端下载")

客户端下载：[cwRsync_4.0.5_Installer.zip](https://pan.baidu.com/share/link?shareid=903891290&uk=118355419)

server:122.152.193.160

client:192.168.7.20

### 二. 服务器端

1. ubuntu server 默认已安装rsync，但是rsync服务默认不是启动的，我们要修改下面的文件$sudo vi /etc/default/rsync
RSYNC_ENABLE=false    #false改true

2. 修改配置文件/etc/rsyncd.conf

我们先来查看一下这个文件
$sudo cat /etc/rsyncd.conf
```
#sample rsyncd.conf configuration file
#GLOBAL OPTIONS
#motd file=/etc/motd #登录欢迎信息
#log file=/var/log/rsyncd #日志文件
#for pid file, do not use >/var/run/rsync.pid if
#you are going to run rsync out of the init.d script.
pid file=/var/run/rsyncd.pid #进程编号
#指定rsync发送日志消息给syslog时的消息级别，常见的消息级别是：uth, authpriv, cron, daemon, ftp, kern, lpr, mail, news, security, sys-log, user, uucp, local0, local1, local2, local3,local4, local5, local6和local7。默认值是daemon。
#syslog facility=daemon #执行系统日志记录活动
#自定义tcp选项，默认是关闭的
#socket options=
#以下是模块信息，我们可以创建多个模块
#MODULE OPTIONS
[ftp]
comment = public archive #模块描述
path = /var/www/pub #需要同步的路径
use chroot = yes #默认是yes|true，如果为true，那么在rsync在传输文件以前首先chroot到path参数指定的目录下。这样做的原因是实现额外的安全防护，但是缺点是需要root权限，并且不能备份指向外部的符号连接指向的目录文件。
#max connections=10 #最大连接数
lock file = /var/lock/rsyncd #指定支持max connections参数的锁文件。
#the default for read only is yes
read only = yes #只读选项
list = yes #客户请求时可用模块时是否列出该模块
uid = nobody #设定该模块传输文件时守护进程应该具有的uid
gid = nogroup #设定该模块传输文件时守护进程应具有的gid，此项与uid配合可以确定文件的访问权限
#exclude = #用来指定多个由空格隔开的多个模式列表，并将其添加到exclude列表中。这等同于在客户端命令中使用--exclude来指定模式，不过配置文件中指定的exlude模式不会传递给客户端，而仅仅应用于服务器。一个模块只能指定一个exlude选项，但是可以在模式前面使用"-"和"+"来指定是exclude还是include    #这个我的理解是排除目录中不需同步的文件
#exclude from = #可以指定一个包含exclude模式定义的文件名
#include = #与exclude相似
#include from = #可以指定一个包含include模式定义的文件名
#auth users = #该选项指定由空格或逗号分隔的用户名列表，只有这些用户才允许连接该模块。这里的用户和系统用户没有任何关系。如果"auth users"被设置，那么客户端发出对该模块的连接请求以后会被rsync请求challenged进行验证身份这里使用的 challenge/response认证协议。用户的名和密码以明文方式存放在"secrets file"选项指定的文件中。默认情况下无需密码就可以连接模块(也就是匿名方式)
#secrets file =/etc/rsyncd.secrets #该文件每行包含一个username:password对，以明文方式存储，只有在auth users被定义时，此选项才生效。同时我们需要将此文件权限设置为0600
strict modes = yes #该选项指定是否监测密码文件的权限，如果该选项值为true那么密码文件只能被rsync服务器运行身份的用户访问，其他任何用户不可以访问该文件。默认值为true
#hosts allow = #允许的主机
#hosts deny = #拒绝访问的主机
ignore errors = no #设定rsync服务器在运行delete操作时是否忽略I/O错误
ignore nonreadable = yes #设定rysnc服务器忽略那些没有访问文件权限的用户
transfer logging = no #使rsync服务器使用ftp格式的文件来记录下载和上载操作在自己单独的日志中
#log format = %t: host %h (%a) %o %f (%l bytes). Total %b bytes. #设定日志格式
timeout = 600 #超时设置(秒)
refuse options = checksum dry-run #定义一些不允许客户对该模块使用的命令选项列表
dont compress = *.gz *.tgz *.zip *.z *.rpm *.deb *.iso *.bz2 *.tbz #告诉rysnc那些文件在传输前不用压缩，默认已设定压缩包不再进行压缩
```
然后定义自己的conf文件

![rsyncd.conf](https://raw.githubusercontent.com/puede/tupian/master/1.jpg)

配置完之后,ESC :wq保存退出

3. 创建一个密码文件
![rsyncd.secrets](https://raw.githubusercontent.com/puede/tupian/master/2.jpg)

$sudo vi /etc/rsyncd.secrets

写入 ursy:123

$sudo chmod 0600 /etc/rsyncd.secrets    

4. 启动rsync

sudo /etc/init.d/rsync start

### 三. 客户端
我们再来看一下客户端的操作，一般客户端不需要进行特殊的配置，直接同步即可.

1. 安装rsync

将下载的客户端rsync解压安装在本地,另外必须配置环境变量

2. 创建一个密码文件
$vi tbu/rsync.pwd 输入123，保存 密码要与服务器端设置的一致

chmod 0600 tbu/rsync.pwd

chown 普通用户:普通用户组 tbu/rsync.pwd

3.  测试服务器rsync的连通性

![telnet](https://raw.githubusercontent.com/puede/tupian/master/4.jpg)

出现@RSYNCD: 31.0 等类似文字，则说明客户端连接服务端正常。

![telnet](https://raw.githubusercontent.com/puede/tupian/master/5.jpg)

### 四. 同步文件

双向

rsync -avzP . root@122.152.193.160:/var/www
 
 rsync -avzP src root@122.152.193.160:des

想同步到哪 就在那个目录下面cmder

同步线上的/var/www 目录到本地etc/目录下

rsync -avz root@122.152.193.160:/var/www  tbu/  

 

![tongbu](https://raw.githubusercontent.com/puede/tupian/master/6.jpg)             
### 五. Rsync命令详解

Rsync的命令格式可以为以下六种：
 
　　rsync [OPTION]... SRC DEST

　　rsync [OPTION]... SRC [USER@]HOST:DEST

　　rsync [OPTION]... [USER@]HOST:SRC DEST

　　rsync [OPTION]... [USER@]HOST::SRC DEST

　　rsync [OPTION]... SRC [USER@]HOST::DEST

　　rsync [OPTION]... rsync://[USER@]HOST[:PORT]/SRC [DEST]

　　
对应于以上六种命令格式，rsync有六种不同的工作模式：
 
　　(1)拷贝本地文件。当SRC和DES路径信息都不包含有单个冒号”:“分隔符时就启动这种工作模式。

如：rsync -a /data /backup
 
　　(2)使用一个远程shell程序(如rsh、ssh)来实现将本地机器的内容拷贝到远程机器。当DST路径地址包含单个冒号”:“分隔符时启动 该模式。

如：rsync -avz *.c foo:src
 
　　(3)使用一个远程shell程序(如rsh、ssh)来实现将远程机器的内容拷贝到本地机器。当SRC地址路径包含单个冒号”:“分隔符时启动 该模式。

如：rsync -avz foo:src/bar /data
 
　　(4)从远程rsync服务器中拷贝文件到本地机。当SRC路径信息包含”::“分隔符时启动该模式。

如：rsync -av root@172.16.78.192::www /databack
 
　　(5)从本地机器拷贝文件到远程rsync服务器中。当DST路径信息包含”::“分隔符时启动该模式。

如：rsync -av /databack root@172.16.78.192::www
 
　　(6)列远程机的文件列表。这类似于rsync传输，不过只要在命令中省略掉本地机信息即可。

如：rsync -v rsync://172.16.78.192/www

### 六. 部分错误的解决办法

1.'rsync' 不是内部或外部命令，也不是可运行的程序或批处理文件。
加环境变量
path = e:\Program Files (x86)\cwRsync\bin

2.rsync: failed to connect to 192.168.7.20: Connection timed out (116)
rsync error: error in socket IO (code 10) at clientserver.c(122) [Receiver=3.0.7]
网络通畅，服务器端允许访问端口 873

3.@ERROR: invalid uid nobody
rsync error: error starting client-server protocol (code 5) at main.c(1506) [Receiver=3.0.7]
指定uid gid
uid = 0
gid = 0

4.@ERROR: chdir failed
rsync error: error starting client-server protocol (code 5) at main.c(1506) [Receiver=3.0.7]
path目录配置的正确，得存在
解决：服务器端同步目录没有权限，cwrsync默认用户是Svcwrsync。为同步目录添加用户Svcwrsync权限。
也可以通过 菜单--cwRsyncServer--02. Prep a Dir for Upload 配置目录权限
除完全控制和特殊权限外的所有权限

5.@ERROR: auth failed on module test
rsync error: error starting client-server protocol (code 5) at main.c(1506) [Receiver=3.0.7]
客户端设置
a.在命令上要指定好用户名
b.密码文件只写密码
rsyncd.secrets文件
rsyncpass
c.用户名密码都要正确

6.Unexpected local arg: /cygdrive/d/rsyncBackup
If arg is a remote file/dir, prefix it with a colon (:).
rsync error: syntax or usage error (code 1) at main.c(1218) [Receiver=3.0.7]
不一定是这个路径有问题，可能是--password-file路径中有空格，服务器端没问题，客户端好像不行

7.password file must be owned by root when running as root
continuing without password file
Password:
设置密码访问权限chown.exe可从服务器端拷贝过来
chmod -c 600 /cygdrive/c/etc/rsyncd.secrets
chown administrator /cygdrive/c/etc/rsyncd.secrets
服务器端不设也可以

### 

1. 将dirA的所有文件同步到dirB内，并删除dirB内多余的文件

 $ rsync -avz --delete  dirA/ dirB/

 
2. 将dirA的所有文件同步到dirB，但是在dirB内除了fileB3.txt这个文件不删之外，其他的都删除。

$ rsync -avz --delete --exclude "fileB3.txt"  dirA/  dirB/

3. 将dirA目录内的fileA1.txt和fileA2.txt不同步到dirB目录内。

$ rsync -avz --exclude="fileA1.txt" --exclude="fileA2.txt" dirA/ dirB/
 
4. 将dirA目录内的fileA1.txt和fileA2.txt不同步到dirB目录内，并且在dirB目录内删除多余的文件。

$ rsync -avz --exclude="fileA1.txt" --exclude="fileA2.txt" --delete  dirA/ dirB/

5. 将dirA目录内的fileA1.txt和fileA2.txt不同步到dirB目录内，并且在dirB目录内删除多余的文件，同时，如果dirB内有fileA2.txt和fileA1.txt这两个被排除同步的文件，仍然将其删除。

$ rsync -avz --exclude="fileA1.txt" --exclude="fileA2.txt"  --delete-excluded  dirA/ dirB/

rsyncd.conf

#sample rsyncd.conf configuration file  
#GLOBAL OPTIONS  
#motd file=/etc/motd  
log file=/var/log/rsyncd  
#for pid file, do not use /var/run/rsync.pid if  
#you are going to run rsync out of the init.d script.  
 pid file=/var/run/rsyncd.pid  
syslog facility=daemon  
#socket options=  
#MODULE OPTIONS  
[my_rsync_bk] #名字可以任意命名，只要客户端的rsync命令一致  
        port = 873
        comment = public archive  
        path = /my_rsync_bk   #指定路径，如果没有这个目录自己建。  
        use chroot = no  
        #max connections=10  
        lock file = /var/lock/rsyncd  
        #the default for read only is yes...  
        read only = yes  
        list = yes  
        uid = nobody  
        gid = nogroup  
        #exclude =   
        #exclude from =   
        #include =  
        #include from =  
        auth users = ursy   #rsync连接时的用户名，要和客户端rsync的命令一致  
        secrets file = /etc/rsyncd.secrets #这里是保存密码的地方，如果屏蔽掉就不用密码了  
        strict modes = yes  
        hosts allow = 192.168.7.20 122.152.193.160 123.160.246.142 #运行的客户端ip  
        #hosts deny =  
        ignore errors = yes  
        ignore nonreadable = yes  
        transfer logging = yes  
        log format = %t: host %h (%a) %o %f (%l bytes). Total %b bytes.  
        timeout = 600  
        refuse options = checksum dry-run  
        dont compress = *.gz *.tgz *.zip *.z *.rpm *.deb *.iso *.bz2 *.tbz  
