---
title: Vim笔记
categories: 笔记
tags:
  - 编程
  - 基础
date: 2018-05-10 15:12:27
---
最近在学习docker，顺便整理了一下Vim，其作为linux端的文本编辑器，还是挺有用的。
<!-- more-->
### 基础
- #### 什么是Vim？
	>Vim是从vi发展出来的一个文本编辑器。
- #### vi/vim的三模式
	1. 命令模式(Command mode)
	> 其它mode下，按`esc`进入该模式
	2. 输入模式(Insert mode)
	> Command mode下输入`i`进入
	> - `i`,`a` 当前光标字符前\后处开始输入
	> - `I`,`A` 行首\尾处开始输入
	> - `o`,`O` 行后\前加一行开始输入
	3. 底线命令模式(Last line mode)
	> Command mode下输入`:`,`/`,`?`进入

### Last line mode(底线命令模式)
在Command mode下按`:`进入Last line mode。

#### `:`模式下的指令
- `q`，`q!`  退出，强制退出
- `w`，`w!`  保存，强制保存
- `wq`，`wq!` 保存退出，强制保存退出(注意：`!`在vi/vim中一般都是‘强制’的意思)
- `w [fileName]`  另存为
- `n1,n2 w [fileName]`  将n1行到n2行出入fileName中
- `r [fileName]`  将fileName中的数据拷入。
- `! command`  command指的是shell命令，执行command命令(还不如在开个命令框比较省事。)
- `set nu`，`set nonu`  显示行号，取消行号
- 替换
	> `{作用范围}s/word1/word2/{替换标示}`，表示在作用范围内将word1替换为word2
	> 1. 作用范围
	>	1. `n1,n2` 表示n1行到n2行
	>	2. `n1,+n` 表示n1行与接下去的n行
	>	3. `%` 表示全文
	>	4. ``  空表示当前行
	>n1可用`.`表示为当前行，n2可用`$`表示为最后一行
	> 2. 替换表示`c`
	>`c`表示为需要确认(confirm)是否取代。确认时按下y表示替换，n表示不替换，a表示替换所有，q表示退出查找模式， l表示替换当前位置并退出。^E与^Y是光标移动快捷键。
	>
	>如果需要到最后，n2可以使用`$`表示，

#### `/`,`?`模式(搜索替换)
- `/word`,`?word` 向下，上搜索(与指令`n`,`N`配合使用)

### Command mode(命令模式)
#### 基本操作
- `x`,`X` 向后\前删除
- `nx`,`nX` 向后\前删除n个字符
- `d$`,`d0` 本行中删除当前字符及之后\前的数据
- `dd`,`ndd` 删除当前行，当前行开始向下后删除n行
- `dG`,`d1G` 删除当前及之后\之前的数据 
- `y$,y0,yy,nyy,yG,y1G` 复制，区别等同	`d$,d0,dd,ndd,dG,d1G`
- `p`,`P` 下\上一行
- `u`,`ctrl+r` 撤销，反撤销
- `.` 重复
- `n`,`N`  重复/反重复上一次搜索

- `n <space>`,`n <enter>` 向后移动n个字符/行
- `G`,`gg`，`nG` 移动到最后/第一/第n行，`G`,`gg`相当与`$G`,`1G`

### 注意
1. mac的键盘：
```
Home键=Fn+左方向
End键=Fn+右方向 
PageUP=Fn+上方向
PageDOWN=Fn+下方向 
向前Delete=Fn+delete键
```

