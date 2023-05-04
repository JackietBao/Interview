# Tomcat 

```shell
公司一般为4核8g，32g的磁盘
Tomcat是一个免费的开放源代码的Web应用，轻量级应用服务器，适合中小型企业使用，需要依赖java环境使用jdk。
JVM：java虚拟机

JRE：jave运行环境 

JAVE类库：语言库
```

# Tomcat 使用方案

## 方案一

```shell
Nginx+Tomcat ##反向代理和负载均衡 
```

## 方案二

```shell
Nginx
Tomcat1 Tomcat2 Tomcat3 ##nginx服务器 ----解析静态页面
建议使用Nginx和Tomcat配合，Nginx处理静态，Tomcat处理动态程序
使用tomcat还需要安装JDK，JDK是 Java 语言的软件开发工具包，JDK是整个java开发的核心，jdk版本要进行与tomcat保持一致。
```

# 端口

```shell
8005：这个端口负责监听关闭Tomcat的请求
8009: 通信接口，接受其他服务器转发过来的请求
8080: 建立http连接用。
```

# jdk里面都有什么

```shell
jd堆，最小运行，最大运行，青年代，中年代，老年代，中年代最大。
```

# 取消JVM 的默认DNS缓存时间

```shell
不缓存DNS记录，避免DNS解析更改后要重启java虚拟机
catalina.sh

添加如下内容 CATALINA_OPTS="$CATALINA_OPTS -Dsun.net.inetaddr.ttl=0 -Dsun.net.inetaddr.negative.ttl=0
```

# Tomcat安全优化

```shell
1：修改默认8005端口
2：降低启动用户访问权限，root用户不得启动
3：停掉脚本权限回收，去除其他用户对Tomcat的bin目录下shutdown.sh、startup.sh、catalina.sh的可执行权限
```

# Tomcat如何优化

## 性能方面的优化

```shell
内存优化：-Xms java虚拟机初始化时的最小内存、-Xmx java虚拟机可使用的最大内存
并发优化：调整连接器connector的并发处理能力
缓存优化：tomcat的maxThreads、acceptCount（最大线程数、最大排队数）

上策：让开发人员去优化代码
中策：优化jvm--优化垃圾回收策略，优化catalina.sh配置文件。在该配置文件中添加代码。
下策：加大内存，该项的资金投入较大
下下策：每天0点定时重启tomcat
```

## 安全方面的优化

```shell
降权启动

防止不法分子通过tomcat获得root权限

修改端口号

修改tomcat配置文件server.xml中的http连接器端口号，防止黑客攻击

隐藏服务类型

在server.xml文件中，为connector元素添加server=” ”，注意不是空字符串，是空格组成的长度为1的字符串，或者输入其他的服务类型，在response header中不显示server的信息
```

# 公司有多少台tomcat，怎么发给这些tomcat，怎么做的？

```shell
3.40台，运用ansible批量发送
一个项目大概5台tomcat
使用ansible，先做好免密然后把三台机器配置到主机清单里面然后再使用模式就可以了。
tomcat默认并发量150
```

# tomcat怎么用？

```
Tomcat 是一个开源的 Java Web 服务器，主要用于部署和运行 Java Web 应用程序。使用 Tomcat 可以快速搭建一个 Web 应用，以下是一些基本的使用步骤：

1. 下载Tomcat。
2. 解压下载的压缩包。 
3. 将应用程序部署到Tomcat中。
4. 配置Tomcat的运行环境。
5. 启动Tomcat服务。

更详细的使用方法可以查看 Tomcat 官方文档。
```



# tomcat怎么优化?

```
优化 Tomcat，可以从以下三个方面入手：

1. 调整JVM参数

JVM参数设置不当可能会导致Tomcat性能下降，因此需要调整JVM参数来优化Tomcat。比如调整内存参数，这可以通过-Xms（初始堆大小）和-Xmx（最大堆大小）来实现。

2. 调整Tomcat参数

Tomcat也有很多参数可以调整以优化性能。比如对于HTTP配置，可以调整连接数、线程数、超时时间等参数来提高性能。

3. 优化Web应用程序

Web应用程序的优化也是Tomcat性能优化的重要方面。例如，可以通过减少HTTP请求、使用CDN、压缩资源和静态资源缓存等来改善Web应用程序的性能。

除此之外，还可通过如下方式进一步提升Tomcat性能：

- 使用SSL加速器
- 开启KeepAlive
- 减少Session的复制和序列化
- 调整缓冲区大小等参数

需要根据具体情况进行分析和调整，选择合适的优化方式，才能达到最佳效果。
```

# 用几台tomcat？怎么把包发送到这20台服务器上？

# war包怎么发送到这20台tomcat服务器上？
