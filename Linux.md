# Linux操作系统如何优化

```shell
①不使用root登录，通过sudo授权，使用普通用户登录
②禁用不必要的进程和服务，减少系统负担
③配置yum源，从国内更新下载rpm包
④关闭selinux以及iptables
⑤Linux内核参数优化，sysctl -a 查询出最优内核参数，修改进程能打开的文件句柄数的数量，max-file表示系统级别的，ulimit -n表示进程级别的，这些参数需要写入/etc/sysctl.conf的文件中，执行sysctl -p执行
⑥磁盘方面，删除一些服务更新残留，采用raid磁盘阵列增大磁盘的使用效率
⑦cpu方面，为进程设置资源限制，使用Linux cgroups 来设置进程的CPU使用上限
⑧内存方面，尽量不设置swap分区，如果物理内存使用完了系统会跑的很慢但仍然可以运行，但swap用完了系统会发生错误
```

# CentOS与Ubuntu有什么区别呢?

```shell
Ubuntu更新周期快速，频繁

1.软件包管理系统(重要区别)。
CentOS使用的是redhat的rpm格式和yum管理工具。Ubuntu使用Debian的.deb格式进行管理。

2.Centos非root用户没有sudo权限，如果需要使用sudo权限必须在/etc/sudoers 中加入账户和权限。
在Ubuntu中，一般使用sudo+命令。

3.在线安装软件中，centos使用的是yum命令，而ubuntu中使用的是apt-get命令。

4.防火墙不同
UbuntuD默认使用UFW防火墙，Centos使用的是iptables或者firewalld防火墙。
```

# centos6和centos7的区别

```shell
1、内核版本：CentOS 6使用的是2.6.x内核，而CentOS 7使用的是3.x内核，新内核带来了更好的性能和更多的功能。 

2、系统架构：CentOS 6只支持32位和64位两种架构，而CentOS 7还支持ARM架构。 

3、系统服务管理：CentOS 6使用的是service命令管理系统服务，而CentOS 7使用的是systemctl命令，更加方便和灵活。 

4、文件系统：CentOS 6默认使用的是ext4文件系统，而CentOS 7默认使用的是XFS文件系统，XFS支持更大的文件和更快的速度。 

5、防火墙：CentOS 6使用的是iptables防火墙，而CentOS 7使用的是firewalld防火墙，firewalld更加灵活和易于管理。

6、软件包管理：CentOS 6使用的是yum软件包管理器，而CentOS 7使用的是dnf软件包管理器，dnf更加快速和稳定。
```

# GO 和 Python 有什么区别？

```shell
1、Python是一种动态类型的语言。 GO是静态类型的语言。

2、两种语言的用途。 Python主要专注于Web开发和基于Linux的应用程序管理。Golang是一种系统语言，开发操作系统的语言，GO也可以并且也用于Web开发需求。

3、GO和Python之间非常重要的区别是面向对象。 Python从头到脚都是面向对象的。 但是，GO不是。 GO是强类型的，并且对面向对象的支持非常平庸。
```

# linux操作系统有哪些发行版本？

```shell
Red Hat Linux
Red Hat 国内使用人群最多的 Linux 版本，资料丰富。
```

![img](assets/Linux/2-1P92FZ955U1.jpg)

```shell
Ubuntu Linux
 Debian Linux 可视化页面，容易上手，对硬件的支持非常全面，适合做桌面系统的 Linux 发行版本，免费。
```

![img](assets/Linux/2-1P92F910131I.jpg)

```shell
SuSE Linux
SuSE Linux 在欧洲较为流行。
SuSE Linux 与 Windows 的交互
```

![img](assets/Linux/2-1P92F91030358.jpg)

# linux软链接和硬链接有什么区别？

```
1、路径
2、跨文件系统
3、目录
4、不存在文件
5、删除文件
```



```
1、软链接可以把路径存放至文件，硬链接是文件副本。
2、软链接可跨不同文件系统而链接，硬链接不行。
3、软链接可对目录链接，硬链接不行。
4、软链接可对不存在的文件名进行链接，硬链接必须要有源文件。
5、删除软链接的源文件，软链接失效。但硬链接可用。
```

# cpu负载与cpu利用率有什么区别？

```
CPU利用率：程序在运行某一时间段所占用CPU百分比。
CPU负载：某段时间正在使用和等待使用CPU的平均任务数。
```

# fdisk和gdisk分区的区别？

```
gdisk：划分磁盘GPT格式，硬盘容量＞2T
fdisk：划分磁盘MBR格式。硬盘容量<2T
```

# Centos7的启动流程是什么？

![img](assets/Linux/1216771-20220608092813955-1848785891.png)



# Linux 常用的网络排查指令有哪些？

```shell
1.ss -tanlp都可以；
2.netstat  -tnlp，有些也可以补充，-u是udp协议，-t是tcp协议；
3.ping  ip地址，也可以确定通不通的一个手段，
4.tranceroute ip地址 ，排查地址经过的路径，
5.抓包，tcpdump命令抓包，tcpdump -nn -i eth0抓包eth0网卡的包
```

# 文件查找

```shell
按文件名
从根开始找文件
[root@qfedu.com ~]# find / -name “file2” #从根开始找文件
/root/file2
/var/tmp/file2
[root@qfedu.com ~]# find /etc -name "ifcfg-ens33" #以名字的方式查找 
[root@qfedu.com ~]# find /etc -iname "Ifcfg-ens33" #-i忽略大小写
```

```shell
按文件的大小
[root@qfedu.com ~]# find /etc -size +5M		#大于5M
[root@qfedu.com ~]# find /etc -size 5M		#等于5M
[root@qfedu.com ~]# find /etc -size -5M      #小于5M
[root@qfedu.com ~]# find / -size +3M -a -size -5M  #查找/下面大于3M而且小于5M的文件
-a：add
[root@qfedu.com ~]# find / -size -1M -o -size +80M #查找/下面小于1M或者大于80M的文件
-o：or
[root@qfedu.com ~]# find / -size -3M -a -name "*.txt" #查找/ 下面小于3M而且名字是.txt的文件
```

```shell
按时间查找
[root@qfedu.com ~]# find /opt -mtime +5		#修改时间5天之前
[root@qfedu.com ~]# find /opt -atime +1     #访问时间1天之前
[root@qfedu.com ~]# find . -mtime -2		#修改时间2天之内

[root@qfedu.com ~]# find . -amin +1         #访问时间在1分钟之前
[root@qfedu.com ~]# find /opt -amin -4      #访问时间在4分钟之内
[root@qfedu.com ~]# find /opt -mmin -2      #修改时间在2分钟之内
```

```shell
按文件类型
[root@qfedu.com ~]# find /dev -type f	#f普通文件
[root@qfedu.com ~]# find / -type f -size -1M -o -name "*.txt"

[root@qfedu.com ~]# find /dev -type d	#d目录
[root@qfedu.com ~]# find /etc/ -type d -name "*.conf.d"

[root@qfedu.com ~]# find /etc -type l	#l链接

[root@qfedu.com ~]# find /dev -type b	#b块设备
[root@qfedu.com ~]# find /dev/ -type b -name "sd*"
```

```shell
按文件权限
[root@qfedu.com ~]# find . -perm 644            #.是当前目录    精确查找644  
[root@qfedu.com ~]# find /usr/bin  -perm -4000  #包含set uid
[root@qfedu.com ~]# find /usr/bin  -perm -2000  #包含set gid
[root@qfedu.com ~]# find /usr/bin  -perm -1000  #包含sticky
```

# awk用法

```shell
FS(输入字段分隔符)---一般简写为-F(属于行处理前)
[root@awk ~]# cat /etc/passwd | awk 'BEGIN{FS=":"} {print $1,$2}'
root x
bin x
daemon x
adm x
lp x
sync x
shutdown x
halt x
mail x
[root@awk ~]# cat /etc/passwd | awk -F":" '{print $1,$2}'
root x
bin x
daemon x
adm x
lp x
sync x
shutdown x
halt x
mail x

#注：如果-F不加默认为空格区分！
===============================================================
OFS（输出字段分隔符）
[root@awk ~]# cat /etc/passwd | awk 'BEGIN{FS=":";OFS=".."} {print $1,$2}'
root..x
bin..x
daemon..x
adm..x
lp..x
sync..x
shutdown..x
halt..x
mail..x
======================================================================
1.创建两个文件
[root@awk ~]# vim a.txt
love
love.
loove
looooove


[root@awk ~]# vim file1.txt
isuo
IPADDR=192.168.246.211
hjahj123
GATEWAY=192.168.246.1
NETMASK=255.255.255.0
DNS=114.114.114.114

NR   表示记录编号, 在awk将行做为记录, 该变量相当于当前行号，也就是记录号
[root@awk ~]# awk '{print NR,$0}' a.txt file1.txt
1 love
2 love.
3 loove
4 looooove
5  
6 isuo
7 IPADDR=192.168.246.211
8 hjahj123
9 GATEWAY=192.168.246.1
10 NETMASK=255.255.255.0
11 DNS=114.114.114.114

FNR：表示记录编号, 在awk将行做为记录, 该变量相当于当前行号，也就是记录号(#会将不同文件分开)
[root@awk ~]# awk '{print FNR,$0}' a.txt file1.txt
1 love
2 love.
3 loove
4 looooove
5  
1 isuo
2 IPADDR=192.168.246.211
3 hjahj123
4 GATEWAY=192.168.246.1
5 NETMASK=255.255.255.0
6 DNS=114.114.114.114
===========================================================
RS(输入记录分隔符)
1.创建一个文件
[root@awk ~]# vim passwd
root:x:0:0:root:/root:/bin/bashbin:x:1:1:bin:/bin:/sbin/nologin
[root@awk ~]# cat passwd | awk 'BEGIN{RS="bash"} {print $0}' 
root:x:0:0:root:/root:/bin/
bin:x:1:1:bin:/bin:/sbin/nologin

ORS(输出记录分隔符)
2.对刚才的文件进行修改
[root@awk ~]# vim passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
[root@awk ~]# cat passwd | awk 'BEGIN{ORS=" "} {print $0}'
root:x:0:0:root:/root:/bin/bash bin:x:1:1:bin:/bin:/sbin/nologin
===========================================================
NF：统计字段的个数
[root@awk ~]# cat /etc/passwd | awk -F":" '{print NF}'
7
7
7
7
$NF:打印最后一列
[root@awk ~]# cat /etc/passwd | awk -F":" '{print $NF}'
/bin/bash
/sbin/nologin
/sbin/nologin
/sbin/nologin
/sbin/nologin

将文件合并为一行
[root@awk ~]# cat /etc/passwd | awk 'BEGIN{ORS="" } {print $0}'


把一行内容分成多行
1.首先创建一个文件
[root@awk ~]# vim d.txt
root:x:0:0:root:/root:/bin/bash
[root@awk ~]# cat d.txt | awk 'BEGIN{RS=":"} {print $0}'
root
x
0
0
root
/root
```

# netstat -tcip参数什么意思？

```shell
-t 选项表示只显示 TCP 连接的信息；
-c 选项表示每隔一段时间重新显示一次当前所有连接的状态；
-l 选项表示只显示监听状态的连接；
-p 选项表示显示每个连接对应的进程 ID 和进程名。
```

# 看磁盘的IO用什么命令？

```shell
使用 iostat 命令来查看磁盘的 I/O 状况，包括磁盘读写的速度、I/O 的等待时间、I/O 请求队列的长度
```

# crontab日志路径在哪儿？

```shell
/var/spool/cron/
```
