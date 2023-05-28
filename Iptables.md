# iptables和Firewalld的区别？

```
iptables和firewalld都是linux防火墙的工具，是为了管理和过滤网络的流量
区别：
1、iptables使用所有linux发行版，firewall使用rehat和centos
2、iptables是基于端口和协议，firewalld基于服务的
3、iptables是静态的，设定了规则就不好更改了。firewalld是动态的，可以在应用运行当中进行修改
4、iptables是传统的linux防火墙工具，firewalld是新的防火墙工具
```

# 你说一下，如何用防火墙设置允许某个ip来访问本机22号端口

```shell
centos7中，可以使用iptables防火墙进行设置
[root@iptables-server ~]# iptables -A INPUT -p tcp --dport 22 -j ACCEPT  #放开22号端口
[root@iptables-server ~]# iptables -P INPUT DROP   #将默认所有进来的请求设置为全部拒绝掉
[root@iptables-server ~]# iptables -P FORWARD DROP #将默认所有的转发的规则设置为全部拒绝掉
注意:修改默认规则： 只能使用ACCEPT和DROP
 # iptables -P INPUT DROP      ----拒绝
 # iptables -P INPUT ACCEPT    ----允许
```

```shell
systemctl start firewalld && systemctl enable firewalld && firewall-cmd --permanent --remove-service=ssh && firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.109.105" port protocol="tcp" port="22" accept"
```

