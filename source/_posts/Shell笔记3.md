---
title: Shell笔记(三)
categories: 笔记
tags:
  - 编程
  - 基础
  - shell
date: 2018-05-15 15:23:04
---
这是最后一篇Shell的笔记了，记录的只是一些基础，用于帮助我们能使用Shell，至于深入，只能靠大量的实践以及对大牛代码的阅读模仿，而我觉得，这些够了。
<!-- more-->
# Shell介绍
## 字符串和数组
### 常用字符串指令
- #### 字符串长度
```
len=${#variableName}
```

- #### 字符串截取 
```shell
#!/bin/bash
#${variableName:<pos> <len>}

str ="0123456789"
echo ${str:1} #输出：123456789
echo ${str:1:2} #输出：12
echo ${str:-3} #当<pos>开始位置为负数时，自动变为0。输出：0123456789
```
- #### expr
```
expr index <string> <chars> #查找<chars>首字母在<string>中出现的位置，没有则返回0。
expr substr <string> <pos> <len> #相当于${variableName:<pos> <len>}
expr length <string> #相当与${#variableName}
expr match <string> <regexp> #正则匹配
```
      
### 数组
**bash支持一维数组，但不支持多维数组，并且没有限定数组大小**
- #### 定义
    ```shell
    arrName=(v0 v1 v2 ...)
    ```
    或
    ```shell
    arrName=(
        v0
        v1
        v2
        ..
    )    
    ```
    或者
    ```shell
    arrName[0]=v0
    arrName[1]=v1
    ...
    ```
- #### 获取
    ```shell
    echo ${array_name[2]} #读取下标为2的元素
    echo ${array_name[*]} #读取所有元素
    echo ${array_name[@]} #读取所有元素

    echo ${#array_name[*]} #获取数组长度
    echo ${#array_name[@]} #获取数组长度
    echo ${#array_name[1]} #获取数组中单个元素的长度
    ```
    输出
    ```shell
    value2
    value0 value1 value2 value3
    value0 value1 value2 value3
    4
    4
    6
    ```

## 函数
### 基础
- #### 定义
    格式如下：
    ```shell
    function funcName(){
        commands
        [return value]
    }
    ```
- #### 调用
    ``` shell
    funcName [p1 p2 ...]
    ```
    > 注意
    > 1. 函数调用时不需要加`()`
    > 2. 函数没有`return`时，则将会将最后一条命令运行结果作为返回值
    > 3. 当`return` 非整数时，函数只能作为赋值，不能直接运行，否则报`return: funcNmae: numeric argument required`错误。
    > 4. 可以使用`unset .f funcName` 删除函数定义。
    > 5. 如果你希望直接从终端调用函数，可以将函数定义在主目录下的 .profile 文件中。

- #### 函数参数
    Shell参数获取，与其他语言有点不同，它是直接使用`${n}`来获取。
    具体查看`变量-特殊变量`

- #### Shell文件的包含(引入)
    使用`.`或者`source`关键字声明，格式如下：
    ```shell
    . fileName
    或
    . "fileName"
    或者
    source fileName
    ```
    >注意：不能之间引用目录
