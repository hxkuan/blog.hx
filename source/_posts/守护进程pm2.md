---
title: 守护进程pm2
categories: 笔记
tags:
  - 后端
  - 工具
  - node
  - npm
  - 基础
date: 2018-09-20 10:36:09
---
之前学习node的时候就接触过，怎么说呢，像docker，pm2这种工具不用真的很容易就遗忘。好吧，简单粗暴，我直接写成blog。看自己的blog肯定比看文档来的快。至于pm2是什么，看标题！！
<!-- more-->
## 简单用法
- ### 安装
  ```node
  npm install -g pm2
  ```
- ### 基本指令
  ```node
  pm2 start/restart/reload/stop/delete app.js/id #开启／重启／重载／关闭／删除
  pm2 logs #查看日志
  pm2 list #查看所管进程列表
  pm2 monit #追踪资源运行情况
  pm2 describe id #查看应用详细部署状态,id为应用id，用pm2 list查看
  pm2 web 
  ```
  > 安装开机自启动 在root 权限下 执行如下命令
  
  ```node
  pm2 startup
  pm2 save
  ```
- ### 集群

    开发环境中多以fork的方式启动，生产环境中多用cluster方式启动
   ```shell
   pm2 start app.js -i 2 --name test
   ```
   这表示启动2个并命名为test，在后台以cluster方式运行
   
   **注意：** 当已经有对应应用运行时，该指令只会重启之前应用，而不会改变应用运行模式，若需要改变，必须现把之前的应用delete

## 预定义运行配置文件

### 基本配置
我们可以预定义一个配置文件，然后制定运行这个配置文件，比如我们定义一个文件process.json，内容如下：
```json
{
  "apps": [
    {
      "name": "ANodeBlog",
      "script": "bin/www",
      "watch": "../",
      "log_date_format": "YYYY-MM-DD HH:mm Z"
    }
  ]
}
```
然后可以通过
```node
pm2 start process.json
```
运行这个App。

### 常用属性

```
{
    "apps": {
        "name": "wuwu",                             // 项目名, 可以通过--only name 指定其中一个app启动        
        "script": "./bin/www",                      // 执行文件
        "cwd": "./",                                // 根目录
        "args": "",                                 // 传递给脚本的参数
        "interpreter": "",                          // 指定的脚本解释器
        "interpreter_args": "",                     // 传递给解释器的参数
        "watch": true,                              // 是否监听文件变动然后重启
        "ignore_watch": [                           // 不用监听的文件
            "node_modules",
            "logs"
        ],
        "exec_mode": "cluster_mode",                // 应用启动模式，支持fork和cluster模式
        "instances": 4,                             // 应用启动实例个数，仅在cluster模式有效 默认为fork；或者 max
        "max_memory_restart": 8,                    // 最大内存限制数，超出自动重启
        "error_file": "./logs/app-err.log",         // 错误日志文件
        "out_file": "./logs/app-out.log",           // 正常日志文件
        "merge_logs": true,                         // 设置追加日志而不是新建日志
        "log_date_format": "YYYY-MM-DD HH:mm:ss",   // 指定日志文件的时间格式
        "min_uptime": "60s",                        // 应用运行少于时间被认为是异常启动
        "max_restarts": 30,                         // 最大异常重启次数，即小于min_uptime运行时间重启次数；
        "autorestart": true,                        // 默认为true, 发生异常的情况下自动重启
        "cron_restart": "",                         // crontab时间格式重启应用，目前只支持cluster模式;
        "restart_delay": "60s"                      // 异常重启情况下，延时重启时间
        "env": {
           "NODE_ENV": "production",                // 环境参数，当前指定为生产环境 process.env.NODE_ENV
           "REMOTE_ADDR": "爱上大声地"               // process.env.REMOTE_ADDR
        },
        "env_dev": {
            "NODE_ENV": "development",              // 环境参数，当前指定为开发环境 pm2 start app.js --env_dev
            "REMOTE_ADDR": ""
        },
        "env_test": {                               // 环境参数，当前指定为测试环境 pm2 start app.js --env_test
            "NODE_ENV": "test",
            "REMOTE_ADDR": ""
        }
    }
}
```
### 注意
- pm2 restart 不会重新加载配置文件，重写加载必须start。
- 配置文件也可以是`.js`文件，如：
    ```
    module.exports = {
      apps : [{
        name: "app",
        script: "./app.js"
       ]}
     }
    ```



## 参考
更多pm2内容请参考官方文档：
- http://pm2.keymetrics.io/docs/usage/quick-start
- https://wohugb.gitbooks.io/pm2/content/index.html
- https://github.com/xiongwilee/blog/issues/6