---
layout:     post
title:      Jenkins-Vue
subtitle:   Jenkins-Vue
date:       2019-01-26
author:     z9961
header-img: https://bing.ioliu.cn/v1?d=2&w=1280&h=320
catalog: true
tags: Jenkins
---

Jenkins持续集成部署Vue项目

jenkins+git+Webhooks

环境：windows server 2008

网上的教程大多数都是linux+ssh，和win下有些不同



#### 步骤

1.打开系统管理-全局工具配置，添加node.js的配置

2.打开系统管理-插件管理-Available，安装[NodeJS Plugin](http://wiki.jenkins-ci.org/display/JENKINS/NodeJS+Plugin)

3.创建一个自由风格的job

4.![mark](http://img.aloli.cn/github/20190126/meFoegrxauUb.png)

![mark](http://img.aloli.cn/github/20190126/EzoPSIW9eFyX.png)

类似于之前的	[Jenkins-Springboot](http://www.aloli.cn/2019/01/22/Jenkins-Springboot/)

```CMD
set BUILD_ID=dontKillMe
start npm run serve
exit 0
```

这里的start是异步执行，不然构建超时

---

如果使用build部署的话

![mark](http://img.aloli.cn/github/20190127/bV5BDSILAavy.png)

```
xcopy dist c:\dist /e /d /y
```

/y 直接覆盖 /d 目标为目录  /e 复制文件和目录

nginx:

```
 server {
        listen       80;
        server_name  xxx.cn;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root c:\dist;
            try_files $uri $uri/ @router;
            index index.html;
        }

        location @router {
            rewrite ^.*$ /index.html last;
        }

        location /api {
            proxy_pass http://127.0.0.1:8888/;
        }
		
		......
```



---

如果出现访问网站提示

```
Invalid Host header
```

在vue目录下创建vue.config.js

```vue
module.exports = {
    devServer: {
        disableHostCheck: true,
    }
}
```

