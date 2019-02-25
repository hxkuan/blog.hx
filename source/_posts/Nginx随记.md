---
title: ' Nginx随记'
categories: 笔记
tags:
  - 编程
  - 后端
date: 2017-11-23 18:09:45
---
## 前言
Nginx，nginx，一天到晚的听到后端的同学在讲这个词，感觉不去了解一下自己都有点说不过去了，所以今天还是抽了点空去了解了一下，顺便总结一下加深一下记忆。

什么是Nginx，这个我就不说了，不知道的同学只要记住它是一个**轻量级的服务器**就行了。那有什么用呢？现在主要用的就两方面：反向代理和均衡负载。

那怎么学习它呢？如果不算怎么安装它，其实剩下的就只有怎么配置它了，更准确的说是怎么编写**nginx.conf**这个文件。至于怎么安装，这边就不介绍了。
<!-- more-->

## nginx.conf配置
### 常用指令
```shell
nginx #开启
nginx -s reload #重新加载nginx.conf
nginx -s stop   #关闭nginx
```
### nginx.conf结构图
```nginx.conf
...
events {
    ...
}

http 
{
    server 
    {
        ...
        location [PATTERN]{
          ...
        }
    }

} 
```
### http 的server块
```nginx.conf
    server {
       listen  80;
       server_name loc.hxkuan.com;

       location /images {
            root /Users/macmini/hxkuan_file/assets/;
            index  1.png;
       }
       error_page 404   /icon.png;

       location /json {
            root /Users/macmini/hxkuan_file/assets/;
       }

       location /404 {
            return 404;
       }

       location /daily/ {
            proxy_pass  http://agent.daily.heyean.com/;
       }
       
       location / {
            root /Users/macmini/hxkuan_file/assets/;
       }

    }

```
**注意：**
- 一个http可以有n个server块。
- listen 后面可以跟 访问地址、访问地址：端口号、端口号。
- 当有多个server的listen相同时，Nginx会匹配server_name指令与请求头部的host字段，host可以使用正则。
- 匹配规则，由上往下，先匹配server，再匹配server内部的location
- location的匹配参数最后不要加‘／’，因为加了‘／’之后，最后不加‘／’的url将有可能匹配不到。location的参数可以使用正则。
- 访问资源时，真正的路径为：root值+location的参数值（很坑，特别注意）
- proxy_pass 后面不能加https链接（需要怎么挑战https的，暂时未知）。
- error_page 可以有多个，有404，403等等；location中可以使用return跳转到error_page上。


## 配置反向代理服务器
要想配置反向代理，首先要掌握基本配置规范，基本的反向代理配置很简单，但是如果要仔细配置也可以做到很复杂。
官网给出反向代理的最简单的代码例子。([https://www.nginx.com/resources/admin-guide/reverse-proxy/](https://www.nginx.com/resources/admin-guide/reverse-proxy/))
```nginx.conf
 location /some/path/ {
    proxy_pass http://www.example.com/link/;
}
```
但是在互联网公司你看到的反向代理配置往往是这样的：
```nginx.conf
upstream baidunode {
server 172.25.0.105:8081 weight=10 max_fails=3     fail_timeout=30s;
}
location / {
    add_header Cache-Control no-cache;
    proxy_set_header   Host local.baidu.com;
    proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
    proxy_set_header   X-Real-IP        $remote_addr;
   
    proxy_pass         http://baidunode;
    proxy_connect_timeout 30s;
 }
```
[具体讲解参考：http://www.jianshu.com/p/e98e84a3322f](http://www.jianshu.com/p/e98e84a3322f)