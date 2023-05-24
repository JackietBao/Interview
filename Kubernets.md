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
secret是k8s其中一个资源对象，一般用来存储密码，密钥等小片敏感数据，将密码放入secret可以减少暴露的风险，

引用方式：它把 Pod 想要访问的加密数据存放到etcd中。然后用户就可以通过在 Pod 的容器挂载 Volume 的方式或者环境变量的方式访问到这些 Secret 里保存的信息了。

命令行和yml格式，一般使用yml行

引用方式：

①卷挂载，在创建pod时我们可以将secret的内容也就是键值挂载到容器中的某个目录中，更新secret中的值，容器里面的值会更新

②映射secret的key到指定目录

在卷挂载的前提下，在volumes的下方定义items，在下方再定义key以及相对路径，将我们想要的数据存储在指定的一个目录中

环境变量

以环境变量的方式引用secret中的数据，比如在创建mysql的pod时，我们可以使用mysql中的预定义变量，通过引用环境变量将secret中的数据定义为数据库的密码，但是更新secret中的值，容器里面的值不会更新
```

