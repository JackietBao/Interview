# Tomcat 

```shell
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
8005：监听关闭Tomcat的请求
8009: 接受其他服务器的请求
8080: 建立http连接用
```

# jdk里面都有什么

```shell
jd堆，最小运行，最大运行，青年代，中年代，老年代，中年代最大。
```



# Tomcat如何优化

## 性能方面的优化

```
内存优化：-Xms初始堆大小、-Xmx最大堆大小，将Xms和Xmx设置一样大。避免程序繁琐申请堆空间。设置为物理内存的一半。

并发优化：调整连接器connector并发处理能力。

缓存优化：tomcat的maxThreads、acceptCount（最大线程数、最大排队数）

上策：让开发人员去优化代码
中策：优化jvm--优化垃圾回收策略，优化Catalina配置文件。
下册：加内存，加钱。
下下策：每天0点定时重启Tomcat
```

## 安全方面的优化

```shell
1：修改默认8005端口
2：降低启动用户访问权限，root用户不得启动
3：更改tomcat关闭指令
4、隐藏服务类型
server.xml文件中，为connector元素添加server=" "(空格组成1的字符串)，或者输入其他的服务类型，在reponse header中不显示server信息
```

# 用几台tomcat？怎么把包发送到这20台服务器上？

```http
相似问题：war包怎么发送到这20台tomcat服务器上？
```



```shell
20台，运用ansible批量发送
一个项目大概5台tomcat
使用ansible，做好免密然后在inventory(主机清单)配置三台机器然后再使用模式就可以了。
tomcat默认并发量150
```

