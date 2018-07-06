---
title: 'CSS晋级摘记-变量 '
categories: 笔记
tags:
  - css
  - 编程
  - 基础
date: 2018-07-04 11:28:57
---
只从2017年3月，微软宣布 Edge 浏览器将支持 CSS 变量。
这个重要的 CSS 新功能，所有主要浏览器已经都支持了，从此你会发现原生CSS变得异常强大。
<!-- more-->
## CSS变量

- ### 定义声明
    使用关键字`'--'`申明。格式如下：
    ```
    selector {
        --varName: value;
        ...
    }
    ```
    > 如此，表示在selector中定义变量`--varName`，所以它的作用域就是选择器的生效范围。当selector为`:root`时，表示全局变量。注意：变量只能作为属性值。
    
    > 为何使用`--`关键字？官方解释是避免和less（@）和sass（$）冲突之后的选择。

- ### 使用方式 var()
    - 使用方式：`property: var(--varName[, defultValue])`
    - 注意点：
        1. 第二个参数不处理内部的`逗号`和`空格`，都将作为参数的一部分，如下：
            ```
            var(--font-stack, "Roboto", "Helvetica"); /* 第二参数为"Roboto", "Helvetica" */
            var(--pad, 10px 15px 20px);
            ```
        1. 变量可以声明变量
        1. 如果变量类型是String，可以与其他字符拼接，如：
            ```
            --bar: 'hello';
            --foo: var(--bar)' world';
            ```
        1. 如果变量为数值，则不能与单位直接使用，必须使用calc()。如：
            ```
            --gap: 20;
            margin-top: var(--gap)px; /* 无效 */
            margin-top: calc(var(--gap) * 1px);
            ```
        1. 如果变量值带有单位，就不能写成字符串。
            ```
            /* 无效 */
            .foo {
              --foo: '20px';
              font-size: var(--foo);
            }

            /* 有效 */
            .foo {
              --foo: 20px;
             font-size: var(--foo);
            }
            ```

## other

- ### 与`media`配合使用
```
body {
  --primary: #7F583F;
  --secondary: #F7EFD2;
}

a {
  color: var(--primary);
  text-decoration-color: var(--secondary);
}

@media screen and (min-width: 768px) {
  body {
    --primary:  #F7EFD2;
    --secondary: #7F583F;
  }
}
```

- ### 兼容处理
    - #### 方法一
        > 使用变量之前声明一个默认值，如
    
        ```
        a {
            color: red;
            color: var(--color)
        }
        ```
    - #### 方法二
        > 使用使用@support命令进行检测。
    
         ```
        @supports ( (--a: 0)) {
        /* supported */
        }
    
        @supports ( not (--a: 0)) {
        /* not supported */
        }
        ```
    - #### 方法三
        使用[`post-css`](https://github.com/postcss/postcss)+[`postcss-cssnext `](https://github.com/MoOx/postcss-cssnext)处理

- ### JavaScript处理
    > JavaScript 也可以检测浏览器是否支持 CSS 变量。

    ```js
    const isSupported =
      window.CSS &&
      window.CSS.supports &&
      window.CSS.supports('--a', 0);

    if (isSupported) {
    /* supported */
    } else {
    /* not supported */
    }
    ```

    > JavaScript 操作 CSS 变量的写法如下。

    ```js
    // 设置变量
    document.body.style.setProperty('--primary', '#7F583F');
    // 读取变量
    document.body.style.getPropertyValue('--primary').trim();
    // '#7F583F'
    // 删除变量
    document.body.style.removeProperty('--primary');
    ```
    
    
    