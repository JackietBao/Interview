# zabbix优点有哪些？zabbix缺点有哪些？

```shell
优点：
1、开源,成本低。
2、Server端对设备性能要求低
3、支持设备多，自带模块多。
4、分布式集中管理,有自动发现功能,可以实现自动化监控
5、当监控项比较多,服务器队列拥挤可以采用被动模式,被监控端主动上传数据到监控端。对监控端的负载小。
6、支持APi接口，扩展性强。
```

```shell
缺点：
1、被监控安装 agent,数据存在数据库里, 数据很大,瓶颈主要在数据库
2、报警设置比较多，如果不筛选的话报警邮件会很多；自定义项目的报警需要自己设置，过程繁琐
```

# zabbix的监控模式有几种？有什么区别？

```shell
主动模式、被动模式
被动模式：agent被动等待server抓取数据
主动模式：agent主动发送数据给server
两种模式都是在agent上的配置文件配置
```

# zabbix的分布式监控有什么特点，怎样部署？

```shell
当agent端过多时，server端需要同多个agent端进行交互，server端的压力大。
在server端和agent端中间可部署一个proxy端，proxy端代理收集数据，将数据统一发送给server端，可以减少server端的压力。

步骤：
1、proxy端安装mysql和zabbix-proxy、zabbix-git、zabbix-agent、zabbix-sender

2、创建zabbix库，创建用户并授权，zabbix-proxy初始化数据导入数据库

3、配置代理端的配置文件

4、启动zabbix-proxy
```

# zabbix设置邮箱报警的流程，简述一下？

```shell
1、创建触发器(定义严重级别、关联监控项)
2、创建动作(确定好主机、默认操作步骤的时间、输入命令)
3、visudo给zabbix提权
4、开启邮箱SMIP，记录授权码
5、创建报警媒体类型(绑定邮箱)
6、选择动作→操作、操作细节，关联用户群组和用户、设置发送到邮箱
7、选择管理→用户，设置报警媒介
```

# Zabbix自动发现怎么做的？

```shell
(1)准备被发现的主机
1、部署agent端
2、设置agent配置文件[Server的ip、提权]
3、开启服务
(2)设置自动发现规则discovery[ip范围、更新间隔.. ]
(3)选择动作→操作→操作细节，关联模板
(4)启动动作、查看效果
```

# Zabbix分布式监控有什么优点？

```
1、可扩展性：
单个zabbix服务器无法满足高流量负载，分布式监控将负载分散到多个服务器。

2、可靠性：
主服务器宕机，其他服务器继续监控活动。

3、降低网络负载：
数据聚集在本地服务器上，减少网络传输负担。
```



# zabbix你们都监控什么内容？如何去做？

## 监控MySQL

```shell
监控mysql服务是否存活
1、被监控节点安装zabbix-agent应用
2、创建主机、配置监控项
3、使用zabbix-server端自带模板net.tcp.port[port]
4、创建触发器，表达式设为：值为1运行，值为0不运行
5、创建图形

监控mysql主从复制状态
1、创建自定义key文件在zabbix路径中，获取主从状态信息，统计IO/sql线程是YES数量(shell命令)，重启agent
2、创建监控项
3、创建触发器(值=2，主从正常，值<2，主从失败)
4、创建图形

监控主从是否存在延迟
1、创建自定义key文件在zabbix路径，获取主从延迟信息，SQL_Delay值(shell命令)，重启agent
2、创建监控项
3、创建触发器(判断前一个值，值为0没有延迟)
4、创建图形


```

```shell
监控mysql主从复制状态
1、创建自定义key文件在zabbix路径中，获取主从状态信息，统计IO/sql线程是YES数量(shell命令)，重启agent
2、创建监控项
3、创建触发器(值=2，主从正常，值<2，主从失败)
4、创建图形



1、使用自定义key的方式，先获取主从状态的信息
mysql -uroot  -e "show slave status\G" | grep -E 'Slave_IO_Running: Yes|Slave_SQL_Running: Yes' |grep -c Yes //统计IO/SQL线程是YES的数量；

2、创建自定义key的文件在/etc/zabbix/zabbix.agent.d/status.conf,在文件中创建key为mysql.status
[root@mysql ~]# vim /etc/zabbix/zabbix_agentd.d/status.conf
UserParameter=mysql.status,mysql -u root -e "show slave status\G" | grep -E 'Slave_IO_Running: Yes|Slave_SQL_Running: Yes'  | grep -c Yes
设置完成后重新启动zabbix-agent

3、创建监控项

4、创建触发器
表达式：(值=2，主从正常，值<2，主从失败)

5、创建图形
```

```shell
监控主从是否存在延迟
1、创建自定义key文件在zabbix路径，获取主从延迟信息，SQL_Delay值(shell命令)，重启agent
2、创建监控项
3、创建触发器(判断前一个值，值为0没有延迟)
4、创建图形

1、使用自定义key的方式，先获取主从延迟信息。
获取SQL_Delay的值，不为0这出现主从延迟
[root@mysql-slave ~]# mysql -u root -e "show slave status\G" | grep SQL_Delay | awk -F : '{print $2}'
0

2、创建自定义key的文件在/etc/zabbix/zabbix.agent.d/status.conf,在文件中创建key为mysql.ab
[root@mysql ~]# cat /etc/zabbix/zabbix_agentd.d/status.conf 
UserParameter=mysql.ab,mysql -u root -e "show slave status\G" | grep "Seconds_Behind_Master" | awk -F: '{print $2}'
设置完成后重新启动zabbix-agent

3、创建监控项

4、创建触发器
表达式：(判断前一个值，值为0没有延迟)

5、创建图形
```

```shell
监控mysql吞吐量
1、使用自定义的方式，先获取到mysql吞吐量的信息(监控mysql的插入、查询、删除、更新等)

2、关闭slave、关闭mysql

3、master节点vim /etc/my.cnf添加配置
[mysqladmin]
user=root
password='auska@123'

4、启动mysql、启动slave、

5、master /etc/zabbix/zabbix_agentd.d/asukaeva.sh 脚本

6、授权

7、做软链接

8、master创建对应的key
[root@mysql-master ~]# cat /etc/zabbix/zabbix_agentd.d/status.conf 
UserParameter=mysql_status[*],bash /etc/zabbix/zabbix_agentd.d/asukaeva.sh "$1"
重启zabbix-agent

9、创建监控项
com_insert
com_delete
com_update
com_slow
com_select

10、创建图形
```

```shell
监控mysql的threads_connected
1、slave执行使用自定义key的方式，先获取到threads_connected的信息。
[root@mysql-slave ~]# mysql -u root -e "select count(*) from performance_schema.threads where name like '%connection';" |grep -v count

2、创建自定义key
[root@mysql-slave ~]# cat /etc/zabbix/zabbix_agentd.d/status.conf 
UserParameter=mysql_thread,mysql -u root -e "select count(*) from performance_schema.threads where name like '%connection';" |grep -v count
[root@mysql-slave ~]# systemctl restart zabbix-agent

3、创建监控项

4、创建触发器，根据服务器的性能设置thread_connected的最大值

5、创建图形
```

```shell
监控数据库的TPS
1、使用自定义key的方式，先获取到TPS的值；
计算公式：Com_commit/S+Com_rollback/S

2、创建一个获取tps的脚本
[root@mysql ~]# cat /etc/zabbix/zabbix_agentd.d/tps.sh 
#!/bin/bash
rollback=`mysqladmin -u root extended-status | awk  '/\<Com_rollback\>/{print $4}'`#事务回滚的次数
commit=`mysqladmin -u root extended-status | awk  '/\<Com_commit\>/{print $4}'` #事务提交的次数
s=`mysqladmin status| awk '{print $2}'`  #Mysql运行的总时长
echo $[($rollback+$commit)/$s]

3、脚本授权

4、创建自定义key
[root@mysql ~]# cat /etc/zabbix/zabbix_agentd.d/status.conf 
UserParameter=mysql.tps,bash /etc/zabbix/zabbix_agentd.d/tps.sh
[root@mysql ~]# systemctl restart zabbix-agent

5、创建监控项

6、创建图形
```

## 监控redis

```shell
1、命令行中查看redis数据的监控指标
[root@redis-server ~]# echo -e "info\n quit" | nc 192.168.153.149 "6379" | grep -w redis_version
redis_version:3.2.12
# nc命令测试端口
# grep -w 精确匹配


[root@redis-server ~]# echo -e "info\n quit" | nc 192.168.153.149 "6379" | grep -w redis_version | awk -F ":" '{print $2}'
3.2.12

[root@redis-server ~]# echo -e "info\n quit" | nc 192.168.153.149 "6379" | grep -w os
os:Linux 3.10.0-693.el7.x86_64 x86_64

[root@redis-server ~]# echo -e "info\n quit" | nc 192.168.153.149 "6379" | grep -w os | awk -F ":" '{print $2}'
Linux 3.10.0-693.el7.x86_64 x86_64

2、编[root@redis-server ~]# vim /etc/zabbix/zabbix_agentd.d/redis_monitoring.sh
#!/bin/bash

function redis_status(){
    R_PORT=$1
    R_COMMAND=$2
    (echo -en "INFO \r\n";sleep 1;) | nc 192.168.153.149 "$R_PORT" > /tmp/redis_"$R_PORT".tmp
    REDIS_STATUS_VALUE=$(grep -w "$R_COMMAND" /tmp/redis_"$R_PORT".tmp | cut -d ':' -f2)
    echo $REDIS_STATUS_VALUE
}

function help(){
    echo "${0} + redis_status+ PORT + COMMAND"
}

function main(){
    case $1 in
        redis_status)
            redis_status $2 $3
            ;;
        *)
            help
            ;;
    esac
}

main $1 $2 $3写脚本获取redis的监控项数据
[root@redis-server ~]# vim /etc/zabbix/zabbix_agentd.d/redis_monitoring.sh
#!/bin/bash

function redis_status(){
    R_PORT=$1
    R_COMMAND=$2
    (echo -en "INFO \r\n";sleep 1;) | nc 192.168.153.149 "$R_PORT" > /tmp/redis_"$R_PORT".tmp
    REDIS_STATUS_VALUE=$(grep -w "$R_COMMAND" /tmp/redis_"$R_PORT".tmp | cut -d ':' -f2)
    echo $REDIS_STATUS_VALUE
}

function help(){
    echo "${0} + redis_status+ PORT + COMMAND"
}

function main(){
    case $1 in
        redis_status)
            redis_status $2 $3
            ;;
        *)
            help
            ;;
    esac
}

main $1 $2 $3

3、redis端配置server端ip、主机名称

4、关联自带的监控模板
```

# 监控nginx性能状态

```shell
1、编写脚本、脚本提权

2、创建nginx监控模板

3、创建监控项，key键值关连模板参数

4、创建图形
```



# zabbix的缺点是什么？如何解决

```
缺点：
1、被监控安装 agent,数据存在数据库里, 数据很大,瓶颈主要在数据库
2、报警设置比较多，如果不筛选的话报警邮件会很多；自定义项目的报警需要自己设置，过程繁琐
```

```
解决：
1、使用zabbix的api接口、插件，扩展储存方式
2、参考官方文档、网上教程或者请教有经验的同行
```



# zabbix结合grafana怎么做？

```
1、安装grafana，端口号为3000
2、下载zabbix插件
3、grafana开启zabbix插件
4、配置zabbix数据源
```



# Zabbix电话报警怎么做？

```
1、使用睿象云web平台，在zabbix-server端安装睿象云web插件
2、安装完之后会自动神生成新的用户和报警媒介
3、象云web平台，点击配置、分配策略，关联zabbix
4、找到通知策略→采用什么方式进行通知
5、zabbix添加动作→关联触发器
```

# 用什么接收报警？什么样的监控项进行电话报警？

```
一般用钉钉进行报警，紧急事项进行电话报警。
钉钉报警：
msyql的thread_connected超过预定的阈值
mysql、nginx、redis端口是否断开
电话报警：
mysql服务是否存活
mysql主从复制状态
CPU、内存利用率达到70%
内存空间少于25%
```



# 怎么排查服务器比较卡顿的情况？是什么原因造成的？怎么去解决？

```
(1)原因：
1、CPU使用率可能大于50%
2、网络使用率过高
3、内存使用率过高

解决：
调整资源、硬件配置升级

(2)原因：
1、DDOS攻击
2、大流量冲击
3、机房故障
4、网卡、网线、交换机故障

解决：
1、及时更新系统
2、删除冗余文件和账户
3、定期故障检查
```



# 钉钉报警的报警媒介什么类型的？

```
脚本
```

# 大概有多少个监控项会电话报警？

```
keepalived高可用问题
mysql服务是否存活
mysql主从复制状态
CPU、内存利用率达到70%
内存空间少于25%
```



# 你们公司都接收到什么报警消息，如何处理的？

```
内存占用特别高达到90%，8颗CPU占用达到800%
报警问题：令人头疼的挖矿病毒
刚接触服务器的话，没有太多的经验，当时一看到这个服务器比较卡，当时非常的紧张。先用top命令查看了一下，发现这个并不是我启动的进程，同时我也去询问开发人员，发现也不是他们的进程。
一开始用kill把它杀死掉，但是发现还会自动重启。我又去网上查了一下，得知是中了挖矿病毒，最后经过了一系列的排查，这个挖矿病毒是由普通用户执行起来的，并不是弱用户，我平时操作的是root用户，我就直接把这个用户删了。但是这个用户还会自动重建。然后我用find查找，根据这个进程的名字，找出所在的文件夹，发现有一个计划任务在循环的执行脚本，一直在执行，到最后我把进程杀掉，把这个文件夹以及所在的脚本都删除掉，再把哪个用户删除掉就OK了。
```



```
keepalived高可用问题
keepalived是给nginx做高可用，高可用一般是抢VIP的，但是我做的时候，备用的节点没有抢到这个VIP。看了日志/var/log/message.log日志，也没有排查出来什么问题。发现keepalived主节点用脚本给它停掉了，但是发现进程并没有停掉，所以导致vip不能漂移。最后打开了keepalived系统配置文件，把进程关闭模式改成了全部杀死，这样就能正常关闭了。然后VIP就能正常飘逸了。
```



```
mysql主从失效
zabbix报警设置中，创建自定义key文件在zabbix路径中，获取主从状态信息，统计IO/sql线程是YES数量(shell命令)，值=2，主从正常，值<2，主从失败。
我们zabbix当时就报警了这个错误，当时并不觉得是有问题的。排查好久也没排查出来。最后发现从节点的版本是8.0的，主节点的版本是5.7的。这都是之前的运维人员留下来的坑，为什么不同步呢，我在slave节点上输入命令show slave status\G，可以看到IO/sql的情况是由于sql语句没有执行成功。

问题的原因：mysql版本不一致，个别语法执行不成功，5.7可以创建用户并授权，8.0必须先创建用户才能授权。

解决方法：先停止主从复制，跳过当前事务，再开启主从同步，手动创建用户并授权，保证主从数据一致。


我们还没有把8.0改为5.7，因为该的比较麻烦，所以暂时就这样设置，但是我们后期创建用户会先create user进行创建，就不用grant。
```

