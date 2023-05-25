# k8s有哪些组件？组件的作用分别是什么

```
1、etcd：
存储kubernetes集群的配置数据，pod、节点的信息

2、Apiserver：
kubernetes接口，负责部署、操作

3、controller-manager:
kubernetes控制器，根据控制器配置pod副本数和当前运行的pod实际数量之间的差异，决定启动或删除一个或者多个pod

4、scheduler：
负责提供策略，将新创建的pod分配到集群中最合适的节点上，提高资源利用率

5、kubelet：
每个节点上都有一个kubelet进程，用于管理pod生命周期，会定期检查pod定义描述的要求，确保容器已经启动并保持运行状态

6、kube-proxy：
为service提高网络代理和负载均衡
```



# k8s和docker-compose的区别是什么？

```
kubernetes和docker compose都是容器编排的工具

docker compose是简单的容器编排工具，通过一个yaml文件管理多个docker容器，但是不适合复杂的操作和高可用的要求

kubernetes是复杂和完备的容器编排系统，提供了智能调度、自动扩缩容、服务发现和负载均衡、故障恢复等功能
```

#  k8s的secret资源对象，有什么作用？你是如何使用它的？

```
secret是k8s其中的资源对象，用来存储密码，secret会将密码进行加密，减少风险的暴露
原理：把pod想访问的加密数据放到etcd中，用户可以通过pod容器挂载volume的方式或者环境变量的方式访问secret信息

pod有两种方式去使用secret：
命令行、yml行

引用方式：
1、卷挂载：
创建pod时，我们将secret内容挂载到容器某个目录，更新secret值，容器会更新

2、映射secret的key到指定的目录
卷挂载前提下，volumes下方定义items，在items下方再定义key以及相对路径，将我们的数据存储指定目录

3、环境变量
环境变量的方式引入secret数据
比如在创建mysql的pod，使用mysql预定义变量，通过引用环境变量将secret数据定义为mysql的密码
但是，更新secret值，容器里面的值不会更新
```



# k8s的secret和configmap有什么区别？详细的说一下？

```
configmap和secret都是来存储程序或容器的配置信息。

1、数据类型：
configmap储存纯文本数据，secret储存加密后的数据

2、安全性：
secret储存的数据是加密的，可以安全地储存敏感数据

3、使用场景：
configmap储存程序的配置信息，比如说环境变量等，secret储存敏感信息比如数据库的密码
```

