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
1、编写告警规则(标签、摘要..)
2、企业微信创建告警应用
记录(agent、secrect、企业ID)
3、vim altermanager.yml
定义路由数信息、定义警报接收者信息
4、企业微信开发者接口JS-SDK，下载文件
5、阿里云函数计算
调用Python，把hello word修改下载文件的内容
6、部署代码，生成URL链接
7、JS-SDK可信
```

# 钉钉报警怎么做？

```
1、在钉钉上创建一个机器人(自定义关键词、标签secret、webhook地址)
2、在Prometheus安装钉钉报警的插件，从GitHub上下载
3、将secret和webhook粘贴到插件的配置文件里
4、vim altermanager配置文件
配置钉钉插件的地址(启动钉钉插件加载出来的用tail -f nohup.out查看)
```

