---
layout:            post
title:             "BOM 对象"
date:              2017-05-29 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---
## BOM: Bower Object Model
## window 对象
global对象是单体内置对象，即不依赖宿主环境的对象。而window对象依赖浏览器。我理解的是，window对象，是global的一个子对象。浏览器环境下，global对象指的就是window对象。
```
//下面两种声明几乎相同，都可以用window.a访问。
var a = 1;  //在全局作用域下声明的变量，都属于window对象的属性；不能使用delete操作符删除属性
window.a = 1; //可以用delete操作符删掉属性
```
很多后台管理系统，邮件管理系统会使用多个frame。
如果页面中包含frame，则每个框架都拥有自己的window对象，并且保存在名为frames的集合中。在frames集合中，可以通过数值索引（从0开始，从左至右，从上到下）或者框架名称来访问相应的window对象。每个frame都有一个name属性，也可以通过name属性访问frame。
```
frames 是一个伪数组，另外两个之前提过的伪数组
1. 函数参数中的 arguments 对象
2. js 获得的DOM节点
```
#### frame 之间的互相通信和管理
此外也可以借助下面三个属性访问到各个框架
* top 最外层的window对象
* parent 当前window对象的上一层
* self  当前的window对象
如果一个页面包含多个window对象，那么每个window对象都会包含原生类型的构造函数，并且这些构造函数相互独立，如果跨越框架传值时会出现问题，例如：
instanceof()判断是否为数组，框架穿梭时会出现问题，因为穿梭后的对象不是当前框架的一个实例。可以先转换成字符串再进行操作。

## 窗口位置
各个浏览器提供了screenLeft和screenTop属性，分别用来表示窗口相对于屏幕左边和上边的距离。

## 窗口大小

## 窗口打开
window.open()
四个参数：要加载的URL、窗口目标、一个特征字符串以及一个表示新页面是否取代浏览器历史纪录中当前加载页面的布尔值。通常只需传递第一个参数，最后一个参数只在不打开新窗口的情况下使用。
