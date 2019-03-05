---
layout:     post
title:      live2d
subtitle:   live2d
date:       2019-01-12
author:     z9961
header-img: https://bing.ioliu.cn/v1?d=2&w=1280&h=320
catalog: true
tags: live2d
---



右下角的黑猫是一款Hexo的插件 [hexo-helper-live2d](https://github.com/EYHN/hexo-helper-live2d) 

因为jekyll能用的 [live2d-widget.js](https://github.com/xiazeyu/live2d-widget.js) 文档失效了，所以把Hexo的插件移到了jekyll上



### 步骤

1.新建一个Hexo项目，装上 [hexo-helper-live2d](https://github.com/EYHN/hexo-helper-live2d) 

2.测试好没问题后hexo deploy编译生成public

3.把public中的live2dw文件夹复制到jekyll项目中，把index.html底部的

```javascript
<script src="/live2dw/lib/L2Dwidget.min.js?0c58a1486de42ac6cc1c59c7d98ae887"><	/script><script>L2Dwidget.init({"pluginRootPath":"live2dw/","pluginJsPath":"lib/","pluginModelPath":"assets/","tagMode":false,"debug":false,"model":{"jsonPath":"/live2dw/assets/hijiki.model.json"},"display":{"position":"right","width":150,"height":300},"mobile":{"show":true},"log":false});</script></body>javascript
```

​	复制到jekyll项目中_includes/head.html里



参考文档 [如何给你的Jekyll博客添加可爱的二次元看板娘(Live2D)](https://done.moe/tutorial/2018/08/11/how-to-add-cute-live2d-in-jekyll-blog/)

