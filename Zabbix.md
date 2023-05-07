# zabbix优点有哪些？zabbix缺点有哪些？

```shell
优点：
1、开源,成本低。
2、Server端对设备性能要求低
3、支持设备多，自带模块多。
4、分布式集中管理,有自动发现功能,可以实现自动化监控
5、当监控项比较多,服务器队列比较大时可以采用被动模式,被监控端主动上传数据到监控端。对监控端的负载小。
6、支持APi接口，与其他系统结合。
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

```
1、开启邮箱SMIP，记录授权码
2、创建按
```



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
配置网络发现Network discovery
(1)准备被发现的主机
1、部署agent端
2、设置agent配置文件[Server端口、提权]
3、开启服务
(2)设置自动发现规则discovery
(3)设置自动发现的动作
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

```http
5月8日
```



# zabbix你们都监控什么内容？如何去做？

## 监控MySQL

```
监控mysql服务是否存活
1、被监控节点安装zabbix-agent应用
2、创建主机、配置监控项
3、使用zabbix-server端自带模板net.tcp.port[port]
4、创建触发器，表达式设为：值为1运行，值为0不运行
5、创建图形
```

```shell
监控mysql主从复制状态
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



# zabbix的缺点是什么？如何解决

```
Zabbix 是一款功能强大的开源监控软件，但是它也有一些缺点，如下所述：

1. 安装和配置复杂：安装和配置 Zabbix 需要一定的 Linux 系统管理经验和知识，对于新手来说可能会有一定难度。

2. 存储方式不够灵活：Zabbix 默认数据存储方式为关系型数据库，无法轻松地切换到其他存储方式。

3. 导入数据缓慢：在 Zabbix 中导入大量历史数据时，可能会导致系统性能下降并导致缓慢。

4. 时间同步问题：Zabbix 需要确保监控服务器和被监控设备的时间同步，否则可能会出现时间误差导致数据异常等问题。

解决这些问题的方法如下：

1. 对于安装和配置复杂的问题，可以考虑参考官方文档、网上教程或者请教有经验的同行。

2. 对于存储方式不够灵活的问题，可以考虑使用 Zabbix 的 API 接口或者第三方的存储插件来扩展其存储方式。

3. 对于导入数据缓慢的问题，可以使用 Zabbix 的分布式存储方案或者采用其他解决方案来解决。

4. 对于时间同步问题，可以采用 NTP 协议来确保设备时间同步。
```

# zabbix结合grafana怎么做？

```shell
Zabbix和Grafana是两个流行的开源监控解决方案。可以通过Zabbix的API将监控数据推送到Grafana中，并使用Grafana漂亮的图表展示数据。下面是具体的步骤：

1. 在Zabbix中，创建一个API用户并赋予权限。可以通过"Administration"菜单下的"API"来进行此操作。

2. 在Grafana中安装"Zabbix data source"插件。可以通过"Plugins"菜单来搜索并安装。

3. 在Grafana中，创建一个Zabbix数据源。需要提供Zabbix服务器的URL以及使用API的用户凭证。

4. 创建一个新的图表面板，并在数据源中选择新创建的Zabbix数据源。

5. 将想要监控的主机或项目添加到图表中。可以选择要监控的指标和时间范围等选项。

6. 根据需要对图表进行调整和编辑。可以添加文本、注释或其他图表，或将多个图表组合在一起。

通过这些步骤，您就可以使用Grafana来展示和分析Zabbix的监控数据，对系统进行实时和历史性能分析。
```

# Zabbix电话报警怎么做？

```shell
1、使用睿象云web平台，在zabbix-server端zabbix脚本目录里安装睿象云报警的脚本。
2、在睿象云操作添加报警的状态、级别。
3、zabbix-server端创建动作
```

# 你们公司都接收到什么报警消息，如何处理的？

```shell
我们公司接收到的 Zabbix 报警消息包括但不限于以下的内容：

1. 系统硬件故障或磁盘容量不足；
2. 网络或应用服务的不可用性或出现异常；
3. 安全事件的监测与跟踪；
4. 数据库连接失败或 SQL 错误；
5. 用户请求失败或超时。

对于这些报警，我们采取的处理方法通常如下：

1. 确认该报警是否真实，例如在磁盘容量报警后，检查磁盘使用情况并排查恶意程序等；
2. 将报警信息反馈到相关的责任人员，这样他们就能在第一时间采取措施，防范可能的意外损失；
3. 如果发现疑似攻击行为，立即采取补救措施，例如增强防火墙、监控网络数据流量等等。

总之，我们尽力保证系统的可用性、可靠性和安全性，并保持响应时间尽可能快。
```

# zabbix监控的优点和缺点？

# grafana怎么配置？

# 用什么接收报警？什么样的监控项进行电话报警？

# 电话报警怎么设置？

# 大概有多少个监控项会电话报警？

# 监控多少个监控端？

# 接受过什么电话报警，怎么去解决的？

# 怎么排查服务器比较卡顿的情况？是什么原因造成的？怎么去解决？

# zabbix都用它监控什么内容？怎么做的？

# 钉钉报警的报警媒介什么类型的？

# 你们都接受过什么报警消息？都是怎么解决的？

# 都监控nginx什么参数怎么做呢？
