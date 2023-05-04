# jenkins工作流程

![image-20230424212405096](assets/Jenkins/image-20230424212405096.png)

# jenkins参数化构建是什么？怎么做？

```txt
1、概念：
程序员发布代码前，要给代码打标签，这个是为了我们可以进行切换版本。要是版本有bug，可以进行版本回退。

2、步骤
(1)、Gitlab代码仓库准备代码，Jenkins安装插件"Git parameter"
(2)、进入jenkins界面，点击"配置"，选择"参数化构建"这个选项，选择你想要的类型。
```

# jenkins自动化触发构建是什么？怎么做？

```
1、概念：
gitlab代码仓库更新代码，jenkins同步更新。适用于更新频繁场景。

2、步骤：
(1)、配置jdk和maven(编译)
(2)、安装Gitlab hooks plugins插件
(3)、新建Gitlab webhook项目
(4)、拉取地址，配置公私钥→免密
(5)、构建触发器
(6)、配置Gitlab
```

# jenkins安装什么插件？为什么要安装这些插件？

```
1. Git Plugin：从Git仓库拉包
2、Maven Integration Plugin：自动化maven构建，maven自动安装、升级
3、 Build Timeout Plugin：停止构建。设置超时时间，避免浪费资源。
4、Email Extension plugin：邮件通知插件
```

# jenkins打成什么包？

```
打包成war文件以便在web应用服务器部署
```

# jenkins用什么工具进行打包？

```
1. Maven：Maven 可对 Java 项目进行构建、文档生成等操作。
2. Gradle：Gradle 类似 Maven，可以进行 Java 项目的构建、依赖管理等。
3. Ant：Ant 是一个 Java 项目的构建工具，类似于 Make，可以进行编译、打包等操作。
```

# jenkins安装ansiber的插件吗？

```
要使用ansible进行自动化构建，需要安装ansible插件。
```

```http
5月3日
```

# 你们都用jenkins做什么？

# 把java语言打成什么包？发到哪里？

# jenkins怎么做参数化构建？

# 参数化构建的作用？
