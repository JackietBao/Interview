# 如何监控远程Linux主机？

```
1、安装node_exporter
2、配置systemctl管理
3、Prometheus-server配置监控
vim prometheus.yml，slave端的ip和端口
```



# Prometheus邮箱报警怎么做？

```
1、开启smtp
2、prometheus-server部署altermanager
3、vim /altermanager.yml
全局配置、定义路由数信息、定义警报接收者信息
关联到altermanager所在机器的ip和端口
指定报警规则的路径
4、配置systemctl系统管理
5、编写告警的规则
```

# prometheus遇到过什么问题？

```
自定义告警模板时间不一致
解决：
startsat.format 后面的时间格式修改为go语言诞生的时间
2006-01-02  15:04:05
```

# 企业微信报警怎么做？

```

```

