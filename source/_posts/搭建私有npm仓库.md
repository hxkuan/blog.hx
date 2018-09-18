---
title: 搭建私有npm仓库
categories: 笔记
tags:
  - node
  - npm
  - 基础
  - docker
date: 2018-09-18 14:19:22
---
**首先，感谢大神rlidwka提供了sinopia，使得私有npm只要简简单单的就能搭建成功。**sinopia（[GitHub](https://github.com/rlidwka/sinopia)）是什么，这边就不在解释了，有需要的自己去github上面看，下面简单的说明一下使用。
<!-- more-->
其实，去年的时候就想搭建一个私有的npm仓库，不过由于本身是android开发（虽然现在致力于ReactNative开发了），加之前端的同学也有这方面的需求，因此果断无节操的坐"等"其成。不幸的是，前端同学换了一批，导致没开始用就被放弃了。好吧靠人不如靠自己，决定自己来。近几个月，由于需要手机端自动化打包，也就被分配了一台机子，最后的条件终于到齐了。

## 部署

### 安装
首先，你要自己配置nodejs及npm的环境，然后运行
```shell
npm install -g sinopia
```
### 启动
```shell
$ sinopia
 warn  --- config file  - .....\AppData\Roaming\sinopia\config.yaml
 warn  --- http address - http://localhost:4873/
```
然后打开：http://localhost:4873/
安装成功画面如果能正常显示，说明安装成功。简单的说，私有npm仓库到此就已经完成了（再次感谢大神rlidwka），只不过都是默认的配置。

### 配置解析
config.yaml
```config.yaml
#
# This is the default config file. It allows all users to do anything,
# so don't use it on production systems.
#
# Look here for more config file examples:
# https://github.com/rlidwka/sinopia/tree/master/conf
#

# path to a directory with all packages
storage: ./storage  //npm包存放的路径

auth:
  htpasswd:
    file: ./htpasswd   //保存用户的账号密码等信息
    # Maximum amount of users allowed to register, defaults to "+inf".
    # You can set this to -1 to disable registration.
    max_users: -1  //默认为1000，改为-1，禁止注册

# a list of other known repositories we can talk to
uplinks:
  npmjs:
    url: http://registry.npm.taobao.org/  //默认为npm的官网，由于国情，修改 url 让sinopia使用 淘宝的npm镜像地址
    
packages:  //配置权限管理
  '@*/*':
    # scoped packages
    access: $all
    publish: $authenticated

  '*':
    # allow all users (including non-authenticated users) to read and
    # publish all packages
    #
    # you can specify usernames/groupnames (depending on your auth plugin)
    # and three keywords: "$all", "$anonymous", "$authenticated"
    access: $all

    # allow all known users to publish packages
    # (anyone can register by default, remember?)
    publish: $authenticated

    # if package is not available locally, proxy requests to 'npmjs' registry
    proxy: npmjs

# log settings
logs:
  - {type: stdout, format: pretty, level: http}
  #- {type: file, path: sinopia.log, level: info}

# you can specify listen address (or simply a port) 
listen: 0.0.0.0:4873  ////全访问，默认没有，只能在本机访问，添加后可以通过外网访问。
```
基本介绍已经备注在文档中了，暂时也不需要使用的那么细致，也就不深入了.
配置完成后再运行us：`sinopia -c config.yaml`

## 使用
### 设置registry
等启动后，
```
npm set registry http://<ip>:4873/
```
将npm的registry只想自己的库，就能publish，install库了，强大之处是sinopia具有`支持配置上游registry配置，一次拉取即缓存`，简单的说就是有匹配，就直接从本地下载，没有，就从上游registry中拉取并缓存。
如果嫌弃设置麻烦，也可以使用`nrm`来切换npm的registry。

### 使用nrm切换registry
- #### 安装nrm:
全局安装nrm可以快速修改,切换,增加npm镜像地址。
```shell
npm install -g nrm # 安装nrm
nrm add XXXXX http://XXXXXX:4873 # 添加本地的npm镜像地址
nrm use XXXX # 使用本址的镜像地址
```
- #### nrm的其他命令：
```shell
nrm --help  # 查看nrm命令帮助
nrm list # 列出可用的 npm 镜像地址
nrm use taobao # 使用`淘宝npm`镜像地址
```

### 创建用户与发布包
1. 但使用本地的registry之后，就可以通过`npm adduser`来注册账号（注意：配置中max_users不能为-1），如果已经有账号，直接使用`npm login`即可。
2. 运行`npm publish`发布新包。

## 结束语
- 以上的情况并没有考虑在遇到一些黑客攻击的情况下，所以为了尽量保证代码的安全，可以在前端加一层 Nginx 然后配置 SSH 公钥来作为双层验证。
- 本人将其写成docker的形势，这样也方便移植，具体的使用查看镜像[hx-sinopia](https://hub.docker.com/r/huangxiangkuan/hx-sinopia/)，注意的是pull的时候需要加上tag。
