---
title: 更好用的侧边栏工具-----better-sidebar
date: 2020-10-06 14:08:30
categories: 组件库
tags:
- vue 
- package
- vue组件库
---


&emsp;&emsp;可能是我眼界较小，没有在市面上见过类似的侧边栏工具，所以想封装一个这样的组件。也是受我们官网的启发，效果如下：

![image](https://qjprod-images.oss-cn-beijing.aliyuncs.com/menuResource/1601956097623?name=44444.gif)


### 封装思路
1. 由两个组件组成，父组件用来包裹，子组件用来放具体内容（受`element-ui`组件库的`时间线`的启发）
2. 支持位置自定义，返回顶部按钮可选
3. 子组件`better-sidebar-item`可以自定义图标，标题，弹层，跳转链接
4. 弹层内容通过插槽引入
5. 



### 最终效果

![image](https://img-blog.csdnimg.cn/20201006140458940.gif)



目前就能想到这么些东西，封装难度并不大，下面是使用说明：

<br/>

### 文档地址

[地址](https://www.liuguisheng.vip/better-sidebar/)


# better-sidebar(侧边栏工具)

### 下载依赖

```
npm i better-sidebar --save
```
<br/>

### 引用

```
import Vue from "vue";
import BetterSidebar from "better-sidebar";
import "better-sidebar/dist/lib/better-sidebar.css";

Vue.use(BetterSidebar);
```
<br/>

### better-sidebar 组件介绍

属性 | 类型 | 可选值 | 默认值 | 描述
-- | -- | -- | -- | --
top | number | - | 100 | 侧边栏距离浏览器顶部的位置
position | string | left/right | right |侧边栏的位置
topButton | boolean | true/false | true | 是否显示返回顶部按钮

<br/>

### better-sidebar-item 组件介绍

属性 | 类型 | 可选值 | 默认值 | 描述
-- | -- | -- | -- | --
icon | string | - | - | 图标字体(该依赖不提供,需要自行下载,引入项目中)
title | string | - | - | 文字
popver | boolean | true/false | false | 是否显示弹出层（内容通过`slot`插入）
link | string | - | - | 点击跳转到新的页面地址

<br/>


### 使用

```
<template>
    <better-sidebar>
      <better-sidebar-item
        class="cell-box"
        icon="icon-gonggao iconfont"
        title="公告"
        popver="true"
        >插槽</better-sidebar-item
      >
      <better-sidebar-item
        class="cell-box"
        icon="icon-biaoge iconfont"
        title="统计"
        link="https://www.liuguisheng.vip"
      />
      <better-sidebar-item
        class="cell-box"
        icon="icon-huodong iconfont"
        title="活动"
      />
    </better-sidebar>
</template>

```


<br/>

### 开源协议
[MIT License](https://github.com/qisi007/better-sidebar/blob/main/LICENSE)

<br/>

### github地址
[地址](https://github.com/qisi007/better-sidebar)

