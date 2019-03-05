---
layout:     post
title:      Vue-Router
subtitle:   Vue-Router
date:       2019-03-04
author:     z9961
header-img: https://bing.ioliu.cn/v1?d=2&w=1280&h=320
catalog: true
tags: Vue
---





用户打开网站时检查token是否存在,不存在则跳转登录界面

```
//使用钩子函数对路由进行权限跳转
router.beforeEach((to, from, next) => {
    if (to.meta.requireAuth) {  // 判断该路由是否需要登录权限
        if (store.state.token) {  // 通过vuex state获取当前的token是否存在
            next();
        } else {
            next({
                path: '/login',
                query: {redirect: to.fullPath}  // 将跳转的路由path作为参数，登录成功后跳转到该路由
            })
        }
    } else {
        next();
    }
})
```



```js
{
            path: '/',
            meta: {
                requireAuth: true // 添加该字段，表示进入这个路由是需要登录的
            },
            component: resolve => require(['../components/views/Home.vue'], resolve),
            children: [
                {
                    path: '/',
                    meta: {
                        title: '系统首页'
                    },
                    component: resolve => require(['../components/views/Dashboard.vue'], resolve),
                },
```



这种情况下requireAuth拿到的是undefined

因为进入的是Dashboard.vue而不是Home.vue

改为

```js
{
            path: '/',
            component: resolve => require(['../components/views/Home.vue'], resolve),
            children: [
                {
                    path: '/',
                    meta: {
                        requireAuth: true, // 添加该字段，表示进入这个路由是需要登录的
                        title: '系统首页'
                    },
                    component: resolve => require(['../components/views/Dashboard.vue'], resolve),
                },
```

