---
title: '设计模式MVC,MVP和MVVM简介'
categories: 编程
tags:
  - 基础
  - 编程
date: 2017-11-07 15:34:26
---
最近在整理之前的笔记，打算经营一下自己的blog。看着自己的笔记，很多当时流行的技术，现在变得很low，还真有一种时过境迁的感觉。不过，技术每时每刻在革新，但一些经典的东西还是那样的经典，比如MVC，理解简单，使用简单，精通的使用却非常的难！
<!-- more-->
总结
1. mvc和mvp的最大区别在于前者不是单线，二后者是单线形式
2. mvp和mvvm的区别就在于view和viewModel是否自动关联
### MVC
MVC模式的意思是，软件可以分成三个部分。
![image](/images/mvc.png)
  1. View 传送指令到 Controller
  2. Controller 完成业务逻辑后，要求 Model 改变状态
  3. Model 将新的数据发送到 View，用户得到反馈


### MVP
MVP 模式将 Controller 改名为 Presenter，同时改变了通信方向。
![image](/images/mvp.png)
1. 各部分之间的通信，都是双向的。
2. View 与 Model 不发生联系，都通过 Presenter 传递。
3. View 非常薄，不部署任何业务逻辑，称为"被动视图"（Passive View），即没有任何主动性，而 Presenter非常厚，所有逻辑都部署在那里。

### MVVM
MVVM 模式将 Presenter 改名为 ViewModel，基本上与 MVP 模式完全一致。
![image](/images/mvvm.png)
唯一的区别是，它采用双向绑定（data-binding）：View的变动，自动反映在 ViewModel，反之亦然。Angular和 Ember 都采用这种模式。