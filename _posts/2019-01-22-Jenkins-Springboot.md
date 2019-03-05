---
layout:     post
title:      Jenkins-Springboot
subtitle:   Jenkins-Springboot
date:       2019-01-22
author:     z9961
header-img: https://bing.ioliu.cn/v1?d=2&w=1280&h=320
catalog: true
tags: Jenkins
---

Jenkins持续集成部署Spring Boot项目

jenkins+git+springboot+Webhooks

环境：windows server 2008



### 步骤

1.[官网](https://jenkins.io)下载安装，配置好密码端口等信息，安装推荐的插件

2.打开系统管理-全局工具配置，添加本地路径：jdk(把自动安装的钩去掉)、git、maven，保存应用

3.回到首页，创建一个新的job，类型选择![mark](http://img.aloli.cn/github/20190123/J5GbndbJWocR.png)

4.![mark](http://img.aloli.cn/github/20190123/zytzTSPepRGm.png)

​	丢弃旧的构建防止硬盘被塞满，保留最后的5个版本

![mark](http://img.aloli.cn/github/20190123/wI2cW7BVAfMU.png)

​	URL填写github仓库的地址

​	点击Add按钮，填写你的github账号和密码，然后选择你的账号

![mark](http://img.aloli.cn/github/20190123/mcaFa3ccGM1g.png)

​	在什么时候触发构建：

​		第二个是Webhooks，即你push到github就触发；

​			[点这里](http://www.aloli.cn/2019/01/22/webhook/)

​		第五个是定时查询github仓库是否有变化，有则触发。

​			15,30,45 * * * *

​			=>在每个小时的15,30,45分的时候定时触发

​	![mark](http://img.aloli.cn/github/20190123/nv7vJ23wBpar.png)

​	构建之前删除之前的工作空间

![mark](http://img.aloli.cn/github/20190123/dSDq43u5Qbt6.png)

点击Add build step添加批处理、maven、批处理

​	第一步先结束掉正在跑的项目，我这里设置的端口号是8888

```CMD
@echo off
for /f "tokens=1-5" %%i in ('netstat -ano^|findstr ":8888"') do taskkill /pid %%m /f
exit 0
```

​	**exit 0必须要有，否则会认为批处理执行失败**

​	第二步构建项目，这里指定了maven仓库的路径

```
clean install -Dmaven.test.skip=true -Ptest -Dmaven.repo.local=c:\maven\m2repository
```

​	第三步运行项目

```
set BUILD_ID=dontKillMe
@echo off
start javaw -jar .\target\XXX-0.0.1-SNAPSHOT.jar
exit 0
```

​	**设置BUILD_ID=dontKillMe避免运行jar的进程被jenkins结束掉**

​	XXX-0.0.1-SNAPSHOT.jar是打包好的jar包的名字，不知道的话在本地打包看看是什么名

保存应用

5.点开创建的job，点击立即构建

​	因为需要到maven仓库下载jar包，这个过程很容易失败

![mark](http://img.aloli.cn/github/20190123/G57WsFldp1hC.png)

​	点击序号进入详情

![mark](http://img.aloli.cn/github/20190123/rivwOGtiYoDs.png)

​	点击Console Output查看输出，如果出现

```
[ERROR]Non-resolvable import POM: Could not transfer artifact XXX:pom:2.27 from/to central (https://repo.maven.apache.org/maven2): GET request of: XXX from central failed
```

​	就是下载失败，重新点击立即构建直到不在报错

​	如果遇到批处理没有执行的情况，可能是权限不足的问题

​		打开服务(services.msc),找到jenkins,右键属性-登录，选择此账号，点击浏览找到管理员账号输上密码，确定然后重启jenkins的服务