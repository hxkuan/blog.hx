---
title: css基础摘记-选择器
categories: 笔记
tags:
  - 编程
  - 基础
  - css
date: 2018-05-20 11:32:48
---
css由【选择器】和【声明】组成
```
selector {declaration1; declaration2; ... declarationN }
```
而【声明】由【属性】和【属性值】组成
```
selector {property: value; ...}
```
在这里，我整理了一下selector部分，相对于庞大的declaration部分，selector的内容其实不多。
<!-- more-->

## 引入CSS
- ### 外部样式表
```
<link rel="stylesheet" type="text/css" href="..." />
```
- ### 内部样式表
```
<style type="text/css">..</style>
```
- ### 内联样式
    >使用`style`属性

## 选择器
我将选择器分为【原选择器】和【组合选择器】前者可单独使用，而后者不能单独选用，只能与原选择器配合使用。

### 原选择器

- #### 元素选择器
    >使用【html元素】作为选择器
    
- #### class选择器
    >表示作用于对应的class上；使用`.`声明
    
- #### id选择器
    >表示作用于对应的id上；使用`#`声明;
    >
    >注意`#`其实是`*#`的缩写，该选择器的完整写法是`selector#idname{}`表示匹配selector中id为idname的元素。class选择也同理。

### 组合选择器

- #### 属性选择器
    >表示作用于拥有某属性的选择器；格式：`a[href][title]{}`
    
- #### 组选择器
    >表示共享【声明】；格式 `h1,h2,h3,h4,h5,h6 {}`
    
- #### 派生选择器
    - ##### 后代选择器：`h1 em{}`
    - ##### 子选择器： `h1>em{}`
    - ##### 相邻选择器
        >表示紧接在另一元素后的元素；`h1+h1{}`
        
- #### 伪类
    >作用于标签，常用的有`:active,:focus,:hover,:link,.visited,:first-child,:lang`

- #### 伪元素
    >作用于标签中的内容，常用的有`:first-letter,:first-line,:before,:after`
    
## 摘要

- CSS外表用`<link>`,内标用`<style>`,内联用`style`
- `'.'`、`'#'`、 `','`、 `' '`、 `'>'`、 `'+'`分表表示class，id，组，后代，子代，相邻
- 伪类作用于标签，伪元素用于内容，css3中伪类用`':'`,伪元素用`'::'`。