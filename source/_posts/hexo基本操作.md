---
title: hexo简介
categories: other
tags:
  - other
  - 生活
date: 2017-10-10
---
链接：
[Hexo官网](https://hexo.io/)
[官方文档](https://hexo.io/docs/)
[hexo 官方GitHub](https://github.com/hexojs/hexo/issues)

## 什么是 Hexo？
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
<!-- more-->
到这里，我们已经了解了如何配置一个Hexo博客，并且通过
```
## 基本操作
$ hexo n ‘name’
$ hexo g
$ hexo s
$ hexo d
$ hexo s -g
```
几个常见命令来新建、更新、预览、同步、更新预览你的博客。
这一节，我们来分享一下Hexo里关于分类和标签的设置技巧。


### 初始化项目

``` bash
$ hexo init <folder>
```

项目文档如下

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

[更多信息](https://hexo.io/docs/setup.html)

### 新建一个页面
首先，我们通过hexo n "name"命令来新建一个页面，在source/_posts目录下找到刚才新建的name.md文件。
我们看到默认的页面是这样的：
```
title: name
date: 2014-08-05 11:15:00
tags:
---
```
可以编辑标题、日期、标签和内容，但是没有分类的选项。我们可以手动加入categories:项，或者打开scaffolds/post.md文件，在tages:上面加入categories:,保存后，重新执行hexo n 'name'命令，会发现新建的页面里有categories:项了。

scaffolds目录下，是新建页面的模板，执行新建命令时，是根据这里的模板页来完成的，所以可以在这里根据你自己的需求添加一些默认值。

### 设置分类列表
在我们编辑文章的时候，直接在categories:项填写属于哪个分类，但如果分类是中文的时候，路径也会包含中文。

比如分类我们设置的是：
```
categories: 编程
```
那在生成页面后，分类列表就会出现编程这个选项，他的访问路径是：*/categories/编程

如果我们想要把路径名和分类名分别设置，需要怎么办呢？

打开根目录下的配置文件_config.yml，找到如下位置做更改：
```
# Category & Tag
default_category: uncategorized
category_map:
	编程: programming
	生活: life
	其他: other
tag_map:
```
在这里category_map:是设置分类的地方，每行一个分类，冒号前面是分类名称，后面是访问路径。

可以提前在这里设置好一些分类，当编辑的文章填写了对应的分类名时，就会自动的按照对应的路径来访问。
### 设置标签
在编辑文章的时候，tags:后面是设置标签的地方，如果有多个标签的话，可以用下面两种办法来设置：
```
tages: [标签1,标签2,...标签n]
```
```
 tages:
- 标签1
- 标签2
...
- 标签n
```

### 绑定git

``` bash
deploy:
    type: git
    repository: https://github.com/hxkuan/hxkuan.github.io.git
   branch: master
```
注意：运行deploy 需要hexo-deployer-git包（npm install hexo-deployer-git --save）

### 设置CNAME

1. 在Hexo的themes/~/source目录下创建CNAME文件（如此，每次hexo g都会在／public下创建CNAME文件）
2. 填写自己的域名如hxkuan.com，保存结束
3. 登录DNSPod，先添加域名，然后添加记录，设置如下

``` bash
主机记录	记录类型	线路类型	记录值	MX优先级	TTL
@	CNAME	默认	wsgzao.github.io.	-	10
www	CNAME	默认	wsgzao.github.io.	-	10