---
title: Chrome开发者工具远程调试Android原生WebView
categories: 笔记
tags:
  - debug
  - 编程
  - 基础
date: 2018-08-13 11:54:21
---
Android Studio 有一套完整的断点调试功能，带对于WebView中Html的调试确无从下手。再加上Android原生WebView版本的风谲云诡，让调试变得异常复杂。幸运的是，从Android 4.4 (KitKat) 开始，使用 Chrome 开发者工具可以帮助我们在原生 Android 应用中远程调试 WebView 网页内容。
<!-- more-->

## 具体步骤

### 第一步
设置 WebView 调试模式。WebView 类包含一个公共静态方法，作为 Debug 开关：
```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
    WebView.setWebContentsDebuggingEnabled(true);
}
```
> 注意：这个方法兼容至 Android 4.4 及更高版本，并且只需设置一次(在Application中设置即可)，便可应用于项目中的所有 WebView，同时不受 Manifest 文件中 debuggable 属性的影响。

### 第二步
确保 USB 连接的前提下，打开 PC 中的 Chrome 浏览器，输入网址，打开页面：
```
chrome://inspect
```
DevTools 页面的 Devices 菜单页自动显示当前连接的远程设备名和序列号，以及当前原生 App 打开的 WebView 的网页地址，如图：
![image](/images/webview_inspect_1.png)
点击对应网页下方的 inspect 选项便可以进入开发者工具页：
![image](/images/webview_inspect_2.png)
如图所示，网页显示内容和源代码、控制台等都可以看到，供安卓开发人员自由调试。