# Ansible的原理是什么？优点？

```
Ansible是轻量级、开源、自动化运维工具。
原理：通过ssh(或Python)在远程主机工作，使用YAML和Playbooks以及Tasks，管理上千台机器。

优点：
1、操作简单、灵活。
2、playbooks编写简单，好理解
3、ssh协议，有密钥，安全。
```

# Ansible工作流程？

```
1、连接：ssh远程连接主机
2、调用模块：有预定义和自定义模块
3、临时文件：会在主机上生成临时文件，临时文件是储存Ansible的临时信息
和主机的配置信息。
4、命令执行：将任务发送给主机，主机执行任务，结果返回Ansible
```

# Ansible都有哪些模块？有什么作用？

```
- command：远程主机执行命令
- copy：复制文件到远程主机
- file：修改文件的权限
- service：管理系统服务，如开启、暂停、关闭服务
- user：创建、删除用户
- group：管理组
- shell：执行shell命令
```

# 你都用Ansible做过什么？怎么写的

```
1、给远程主机创建用户
ansible weball[主机组] -m shell -a 'useradd asuka'

2、复制文件到远程主机并修改权限(属主、属组)
ansible weball -m copy -a 'src=/root/a.txt dest=/root owner=root group=root mode=644' -o

3、运用playboos引用name、yum变量安装apache、nginx、mysql等
引用name、copy变量、notify触发，将主机apache的配置文件覆盖远程主机上
```

# Ansible都需要配置什么？去实现控制机器

```
1、节点配置
域名解析/etc/hosts、inventory(远程主机ip)/etc/ansible/hosts

2、ssh配置
将主机ssh公钥发送远程机器实现免密
```



