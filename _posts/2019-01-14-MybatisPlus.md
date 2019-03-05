---
layout:     post
title:      MybatisPlus
subtitle:   MybatisPlus
date:       2019-01-14
author:     z9961
header-img: https://bing.ioliu.cn/v1?d=2&w=1280&h=320
catalog: true
tags: MybatisPlus
---





### 遇到的问题

1.MybatisPlus更新的很快，文档更新的不及时，在按照起步文档搭建的时候

[代码生成器](https://mp.baomidou.com/guide/generator.html#添加依赖) 

这里报com.baomidou.mybatisplus.generator找不到

在v3.0.7的更新里

```
移除 对 mybatis-plus-generator 包的依赖,自己按需引入
```



所以需要在pom里添加

```
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.0.7.1</version>
        </dependency>
```



2.[lombok在IntelliJ IDEA下的使用](https://www.cnblogs.com/softidea/p/5960182.html)

​	

