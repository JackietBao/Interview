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



