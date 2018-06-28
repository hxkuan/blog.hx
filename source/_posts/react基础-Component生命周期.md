---
title: react基础-Component生命周期
categories: 笔记
tags:

  - 前端
  - 编程
  - 基础
date: 2018-01-4 14:04:19
---
### 前言
Component，作为react所有组件的基类，其重要性毋庸置疑。而想要更好的使用它，无疑需要清晰的了解其生命周期，否则试问你又如何去开发优化自己的组件呢？下面就是我整理的生命周期。
<!-- more-->
## 生命周期
为了便于记忆，我将其分为4个块（render模块，mount模块，update模块和unmount模块），其中3个模块完全相互独立。
### render 模块
>如果说component中有一个生命周期是最重要的，那无疑是`render()`。之所以将一个方法称为一个模块，是因为一方面它的重要性，另一方面它于mount模块和update模块都有千丝万缕的关系，实在不好将它归于update或者mount模块中。至于其本身的作用，估计也不需要过多的解释了。
>

### mount模块
mount模块有三个方法，依次为：
- constructor(props)
- void componentWillMount()
- void componentDidMount()

>这三个方法只会被调用1次；后两个方法，一个在render之前被调用，一个则在render之后被调用，总的来说这个模块是一个组件初始化到显示的过程。
>
>需要注意的点是componentWillMount虽然在render之前被调用，但并不是在调用完compotentWillMount后调用render，所以即使在compotentWillMount加上async-await也无法做到于render同步。 
>

### update模块
update模块有4个方法，依次为：
- void componentWillReceiveProps(nextProps)
- boolean shouldComponentUpdate(nextProps, nextState)
- void componentWillUpdate()
- void componentDidUpdate()

>先说说componentWillReceiveProps。props是父组件传递给子组件的，父组件发生render的时候子组件就会调用componentWillReceiveProps（不管props有没有更新，也不管父子组件之间有没有数据交换）之后在调用shouldComponentUpdate。所以如果有需要使用props初始化state值时，最正确的做法是同时在constructor中初始化的同时在componentWillReceiveProps中setState。
>
>shouldComponentUpdate方法是在组件mount之后，每次props或者state改变时会被调，该函数用来判断是否需要重新渲染组件。
>
>componentWillUpdate/componentDidUpdate，更新界面前后被调用。这两个周期函数会在shouldComponentUpdate返回true，或者使用this.forceUpdate()时启用。
>

### umount模块
unmount模块只有两个方法，依次为：
- void componentWillUnmount()

>这2个方法也只会被调用一次。一般在componentWillUnmount中注册事件，而在componentDidMount里面注册的事件需要在这里删除
>
>注意：调用componentWillUnmount的时候，虽然整个组件是正常运行状态，但由于非同步调用，所以其实不建议在这里使用操作本组件的方法，正确的做法是先在运行状态上面操作完成后unmount组件。
>

## 生命周期图解
![image](/images/react_lifecycle.png)

