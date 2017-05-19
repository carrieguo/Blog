---
layout:            post
title:             "对象"
date:              2017-05-17 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---
各种属性及其对应值组成的无序合集。

## 数组
#### 创建
```javascript
new Array(2);   //包含两项，每一项都是 undefined
new Array('a'); //
var a = ['a','b'];
```
#### 读取
```javascript
a[0];
```

#### 删除操作
```javascript
delete a[0];
```

#### 检测数组
```javascript
if (list instanceof Array){} 

if(list.constructor == Array){}
//以上两种方法，在多个 iframe 下可能会出错

if(Array.isArray(list)){}   //es5

Object.prototype.toString.call(o) === '[object Array]';
```

#### 方法
##### 栈方法，队列方法
```javascript
var list = new Array();
var length = list.push('a','b');    //推入两项并返回长度
list.pop();

list = list.shift();  //取出并返回第一项
length = list.unshift('a');    //推入，并返回长度
```
##### 重排序
```javascript
list.reverse(); //反转顺序
list.sort();    //先转换为字符串再排序
list.sort(function(a,b){
    return a-b;
});    //sort 是一种冒泡排序
```
sort()方法需要多次计算


