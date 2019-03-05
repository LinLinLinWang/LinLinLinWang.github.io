---
layout:     post
title:      Vue-SpringBoot
subtitle:   Vue-SpringBoot
date:       2019-01-13
author:     z9961
header-img: https://bing.ioliu.cn/v1?d=2&w=1280&h=320
catalog: true
tags: Vue
---





### 前后端分离后数据通信的问题

1.在Vue中使用axios来post和get

​	*npm* *install* --*save* axios

​	然后在main.js中添加

```vue
import axios from 'axios';
```

```vue
axios.defaults.baseURL = 'http://127.0.0.1:8888';  //之后的url直接写/xxx
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'; //改为表单提交
axios.defaults.withCredentials=true; //携带cooki
axios.interceptors.request.use(function (config) {
    // 在发送请求之前,格式化参数，增加token
    let data = config.data;
    let params = new URLSearchParams() //将参数转换为url的形式
    for (var key in config.data) {
        params.append(key, data[key])
    }
    config.data = params;
    return config;
}, function (error) {
    return Promise.reject(error);
});
Vue.prototype.$axios = axios;
```

​	在new Vue(){

​	    }

​	中添加

```vue
	axios,
```

2.在要使用请求的vue中添加methods

```
            testaxios() {
                this.$axios({
                    method: 'POST',
                    url: '/users/login',
                    data: {
                        username: 'uname',
                        password: 'pwd'
                    }
                }).then(response => {
                    var resdata = response.data;
                    this.info = resdata;
                    if (resdata.state == "200") {
                        alert("登录成功\nmsg:" + resdata.msg+"\nstate:"+resdata.state);
                    }
                })
            }
```

​	这里的this.info是template中的{{info}}

3.跨域问题

​	前端地址为localhost:8080

​	后端地址为localhost:8888

​	浏览器默认禁止跨域请求，有多种解决方式

​	

​	对于Spring Boot项目

​	新建一个类即可

```java
@Configuration
public class CORSConf {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")//匹配所有的请求
                        .allowedHeaders("*")    //请求头
                        .allowedMethods("*")    //post,get
                        .allowedOrigins("*")    //允许所有来源
                        .allowCredentials(true); //携带cookie
            }
        };
    }
}


```



附上后端代码

```
@RestController
@RequestMapping("/users")
public class UsersController {

    @Autowired
    private IUsersService usersService;

    @RequestMapping("/getall")
    public List<Users> getall() {
        return usersService.list();
    }

    @RequestMapping("/login")
    public Map<String,String> login(String username, String password) {
        System.out.println(username+"\n"+password);
        Map<String,String> map = new HashMap<>();
        map.put("state","200");
        map.put("msg","ok");
        return map;
    }
}

```



