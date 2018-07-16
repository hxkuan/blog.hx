---
title: react-native 基础-常用css篇
categories: 笔记
tags:
  - 笔记
  - 后端
  - 基础
  - js
date: 2018-04-25 18:18:36
---
RN的样式是CSS的一个子集，为此整理一份常用的RN CSS样式，以此作为备忘录。
<!-- more-->

## React Native常用CSS

根据官网分类，目前可分为：
- 组件样式
- 布局样式
  1. Layout style
  2. Flexbox style
- 效果样式
  1. Transform style
  2. Shadow style

### 组件样式

#### View部分
borderColor, borderTopColor/.., 

borderRadius, borderTopLeftRadius/.., 

borderStyle['solid','dotted','dashed'],

borderWidth, borderTopWidth/..,

opacity, backgroundColor, 

backfaceVisibility['visible','hidden'], overflow['visible','hidden']

#### Text部分
color, fontFamily, fontSize, fontStyle, fontWeight['normal','bold'],
 
lineHeight, letterSpacing, textAlign['auto','left','right','center','justify'],

textShadowColor, textShadowOffset, textShadowRadius,

textDecorationStyle, textDecorationLine, textDecorationColor,

### 布局样式
#### Layoyt样式
width, height,

margin, marginTop/.., marginHorizontal/marginVertical,

padding, paddingTop/.., paddingHorizontal/paddingVertical,

position['absolute','relative'], top/right/bottom/left,

#### FlexBox样式
flex, flexDirection['row','column'], flexWrap['wrap','nowrap'],

alignSelf['auto','flex-start','flex-end','center','stretch'],
alignItems['flex-start','flex-end','center','strench'],
jusityContent['flex-start','flex-end','center','space-between','space-around']

### 效果样式
#### Transform样式
transform [{perspective: number}, {rotate: string}, {rotateX: string}, {rotateY: string}, {rotateZ: string}, {scale: number}, {scaleX: number}, {scaleY: number}, {translateX: number}, {translateY: number}, {skewX: string}, {skewY: string}]
#### Shadow样式
这方面，android其实并不支持Shadow效果，但通过elevation可以模拟出一定的效果。
*(IOS)*
shadowColor, shadowOffset {width: number, height: number}, shadowOpacity, shadowRadius
*(ANDROID)*
elevation
```css
  elevation: 4,
  zIndex: __IOS__ ? 1 : 0
```
