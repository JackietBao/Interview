# shell和python有什么区别？

```
1、语言类型：
shell是命令行解释器，执行操作系统命令和脚本
python是编程语言，可编写应用程序

2、语言特性：
shell脚本用于执行简单的系统任务
python支持面向对象编程、函数式编程和其他高级概念。

3、变量类型：
shell是弱语言
python是强语言

4、编写方式：
shell通常逐行编写，每个命令都解释为一个单独程序
python更多关注程序结构和代码重用性

5、执行效率
shell是解释型语言执行效率低
python使用编辑器和解释器混合技术，执行效率高。
```

# 都写过什么shell脚本？怎么写的？

```shell
#!/bin/bash
echo "系统巡检脚本：Version `date +%F`"

echo -e "\033[33m*******************************************************系统检查 *******************************************************\033[0m"
echo "系统：`uname -a | awk '{print $NF}'`"
echo "发行版本：`cat /etc/redhat-release`"
echo "内核：`uname -r`"
echo "主机名：`hostname`"
echo "SELinux：`/usr/sbin/sestatus | grep 'SELinux status:' | awk '{print $3}'`"
echo "语言/编码：`echo $LANG`"
echo "当前时间：`date +%F_%T`"
echo "最后启动：`who -b | awk '{print $3,$4}'`"
echo "运行时间：`uptime | awk '{print $3}' | sed 's/,//g'`"

echo -e "\033[33m*******************************************************CPU检查 *******************************************************\033[0m"
echo "物理CPU个数: `cat /proc/cpuinfo | grep "physical id" | awk '{print $4}' | sort | uniq | wc -l`"
echo "逻辑CPU个数: `cat /proc/cpuinfo | grep "processor" | awk '{print $3}' | sort | uniq | wc -l`"
echo "每CPU核心数: `cat /proc/cpuinfo | grep "cores" | awk '{print $4}'`"
echo "CPU型号: `cat /proc/cpuinfo | grep "model name" | awk -F":" '{print $2}'`"
echo "CPU架构: `uname -m`"
echo -e "\033[33m*******************************************************内存检查 *******************************************************\033[0m"
echo "总共内存：`free -mh | awk "NR==2"| awk '{print $2}'`"
echo "使用内存：`free -mh | awk "NR==2"| awk '{print $3}'` "
echo "剩余内存： `free -mh | awk "NR==2"| awk '{print $4}'`"
echo -e "\033[33m*******************************************************硬盘检查 *******************************************************\033[0m"
echo "总共磁盘大小：`df -hT | awk '/\/$/' | awk '{print $3}'`"


echo -e "\033[33m*******************************************************网络检查 *******************************************************\033[0m"
echo `ip a | grep eno | awk "NR==2" | awk '{print $NF,":",$2}'`
echo "网关：`ip route | awk 'NR==1'| awk '{print $3}'`"
echo "DNS: `cat /etc/resolv.conf | grep "nameserver" | awk '{print $2}'`"

ping -c 4 www.baidu.com > /dev/null
if [ $? -eq 0 ];then
    echo "网络连接：正常"
else
    echo "网络连接：失败"
fi
echo -e "\033[33m*******************************************************安全检查 *******************************************************\033[0m"
echo "登陆用户信息：`last | grep "still logged in" | awk '{print $1}'| sort | uniq`"
md5sum -c --quiet /md5file/passwd.md5 &> /dev/null  #passwd.md5提前创建好
if [ $? -eq 0 ];then
    echo "文件未被串改"
else
    echo "文件被串改"
fi
```

# 计算机语言的类型有哪些？

```
计算机语言(编译型语言、解释型语言)
编译型：C C++ java
将源代码编译成机器语言，再由机器运行机器码(二进制)
一次编译，重复使用

解释型：bashshell python
编译代码，解释器对代码进行解释运行
```

