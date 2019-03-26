---
title: react v16.3+生命周期改动
categories: 笔记
tags:
  - 基础
  - js
  - react
date: 2018-12-26 16:22:39
---
React 官方在 v16.3 版本更新中，除了`Context API `之外，新引入的两个生命周期函数 `getDerivedStateFromProps，getSnapshotBeforeUpdate` 以及在未来 `v17.0` 版本中即将被移除的三个生命周期函数 `componentWillMount，componentWillReceiveProps，componentWillUpdate`。
<!-- more-->
**注意**：v16.3-V17.0版本中，这两套生命周期不能同时使用，否则会报错“你使用了一个不安全的生命周期”（unsafe）。[查看生命周期](/2018/01/04/react基础-Component生命周期/)

## 一、图对比
### Before v16.3
![before v16.3新生命周期](/images/react-v16.3before_lifecycle.png)
### After v16.3
![after v16.3新生命周期](/images/react_v16.3_lifecycle.jpg)

## 二、函数解析

### getDerivedStateFromProps(nextProps, prevState)
- 是一个静态函数，所以也就无法使用`this`;参数表示`之后的Props`和`之前的State`；
```
static getDerivedStateFromProps(nextProps, prevState) {
    if (nextProps.isLogin !== prevState.isLogin) {
    return {
      isLogin: nextProps.isLogin,
      };
  }
    return null;
}
```

- 每次接收新的props之后都会返回一个对象作为新的state，返回null则说明不需要更新state.

- 配合componentDidUpdate，可以覆盖componentWillReceiveProps的所有用法

### getSnapshotBeforeUpdate(prevProps, prevState)

- 触发时间: update发生的时候，在render之后，在组件dom渲染之前。

- 返回一个值，作为componentDidUpdate(prevProps, prevState, snapshot)的第三个参数。

-  配合componentDidUpdate, 可以覆盖componentWillUpdate的所有用法。


## 三、备注
- componentWillReceiveProps、componentWillMount和componentWillUpdate，在一个轮回中可能会被调用多次；而componentDidMount和componentDidUpdate则只会被调用一次；

- componentWillMount被调用后未必会调用未必会调用componentWillUnmount，而componentDidMount之后肯定会调用。