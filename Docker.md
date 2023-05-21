# Docker的优点有哪些？

```http
Docker给你的工作带来的好处有哪些？
```



```
1、安全性好
要是服务器启动多个服务，那么这些服务相互之间可能会产生影响，要是某一个服务被入侵了，其他的服务可能遭受到影响。docker可以有效避免这样的影响。

2、高效利用系统资源
容器不需要硬件虚拟化，docker对系统资源利用率高。无论是执行速度和文件存储速度，都要比传统虚拟技术高效

3、启动时间更快
传统虚拟机启动应用服务往往需要几分钟，docker运行在宿主内核，无需启动完整操作系统，因此启动快

4、运行环境一致
docker提供完整的运行环境，确保应用环境一致性。避免在开发环境、测试环境、生产环境不一致，导致bug

5、持续交付和部署
运维可直接在生产环境中快速部署该镜像

6、方便迁移
因为docker执行环境一致，所以迁移方便。无论是物理机、虚拟机、公有云、私有云，运行结果是一致的。
```



# 容器的类型只有Docker吗？

```
我所了解到的容器除了docker还有kubernetes
docker和kubernetes的区别？

docker是一个容器化平台和运行环境
kubernetes通过众多容器运行的环境来进行运行和管理容器的一个平台
```



# Docker镜像仓库，公开的和私有的，有什么区别？

```
公开的镜像仓库：在拉取的时候，不需要输入用户名和密码。推送仍然需要输入用户名和密码
私有的镜像仓库：拉取，推送都需要输入用户名和密码
```

# Dockerfile如何优化

```
1、合并RUN指令
多个RUN指令合并成一个，减少构建大小和构建时间

2、构建过程中，使用rm命令删除不必要的文件，减少镜像大小

3、常用文件放前面
常用文件放到前面，有利于加快后续构建速度
```



# Dockerfile常用的指令有哪些？

```
FROM：创建一个基础镜像
MAINTAINER：Dockerfile设置作者信息
RUN：执行命令、安装软件
CMD：容器默认执行命令
EXPOSE：容器使用端口
ENV：设置环境变量
ADD：复制文件或目录到容器中，可自动解压
COPY：将文件或目录复制到容器中
ENTRYPOINT：为容器设置一个入口点
VOLUME：容器将用于存储持久化容器的卷
WORKDIR：为容器设置工资目录
USER：设置容器时的用户或UID
ARG：指定构建参数，列如镜像版本号
ONBUILD：构建过程中镜像设置触发器，构建新的镜像会触发并执行相应命令
```



# Dockerfile中的CMD和ENTRYPOINT指令有什么区别？

```
CDM：
1、每个dockerfile中只能有一个CMD，若是有多个只能执行最后一个
2、如果在运行容器时指定了要执行的命令，那么CDM命令就会被覆盖

ENTRYPOINT
1、指定容器启动时要执行的命令，无论是在容器运行时指定了什么命令，ENTRYPOINT都会被执行
2、Dockerfile存在多个ENTRYPOINT指令，最后一个ENTRYPOINT指令生效
```



# Docker的网络模式有几种，有什么区别？

```
bridge、host、none
bridge：
默认情况下启动、创建容器都是用该模式，每次docker容器重启时会按照顺序获取相应ip地址

host：
容器与宿主机共享一个网络命名空间，不需要端口映射可访问容器的服务

none：
启动容器时，通过--network=none，docker容器不会分配局域网ip
```



# Dockerfile中的ADD和COPY有什么区别？

```
1、copy复制本地文件或目录到文件中，add支持远程url下载

2、add可以自动解压，copy复制本地压缩包，接着使用run手动压缩
```



# Docker如何做资源限制？

```shell
CPU限制
1、cpu限制
docker run --rm -it -c 512 progrium/stress --cpu 4

2、限制cpu的核数
限制cpu只能使用1.5核数cpu
docker run --rm -it --cpus 1.5 progrium/stress --cpu 3

3、cpu绑定
假如主机上有4个核心，可以通过--cpuset参数让容器只能运行在前两个核上
docker run --rm -it --cpuset-cpus=0,1 progrium/stress --cpu 2 

men资源限制
[root@docker-server ~]# docker run --rm -it -m 64m progrium/stress --vm 1 --vm-bytes 64M --vm-hang 0

容器可以正常运行。
-m 64m：限制你这个容器只能使用64M
--vm-bytes 64M：将内存撑到64兆是不会报错，因为我有64兆内存可用。
hang:就是卡在这里。
--vm：生成几个占用内存的进程

而如果申请 150M 内存，会发现容器里的进程被 kill 掉了（worker 6 got signal 9，signal 9 就是 kill 信号）
猜测：如果内存消耗占用限制数值的2倍，容器会被kill掉

[root@docker-server ~]# docker run --rm -it -m 64m progrium/stress --vm 1 --vm-bytes 150M --vm-hang 0
```

