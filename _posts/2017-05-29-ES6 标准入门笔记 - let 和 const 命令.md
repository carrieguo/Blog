---
layout:            post
title:             "let 和 const 命令"
date:              2017-05-29 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---

## let 命令
let 命令，用于声明变量。用法类似于 var, 但是所声明的变量只在let命令所在的代码块内有效。
```javascript
{
    let a = 10;
    var b = 1;
}
a //ReferenceError: a is not defined
b //1
```
#### 块级作用域
在ES5中，作用域链机制使得比闭包只能取得包含函数中任何变量的最后一个值。我们在for循环中借助`立即执行函数表达式(IIFE) `保存了闭包状态。现在我们使用let也可以达到目的。
```javascript
var data = [];
 
for (let i = 0; i<=3; i++) {
  data[i] = function () {
    console.log(i);
  };
}
 
data[0]();  //0
data[1]();  //1
data[2]();  //2
```
    上面的代码中i是let声明的，当前的i只在本轮循环有效。所以每一次循环的i其实都是一个新的变量。
    
#### 不存在变量提升
    let 不像 var 有“变量提升”。所以，变量一定要先声明，再使用。
```javascript
console.log(foo);
let foo = 2;
```
#### 不允许重复声明
let 不允许在相同作用域内重复声明同一个变量。
```javascript
//报错
function func() {
  let a =10;
  var a =1;
}
```
## const 命令
声明常量，必须一次性赋值。
只在声明所在的块级作用域内有效。

## 变量的解构赋值
```javascript
//ES5 写法
var a = 1;
var b = 2;
var c = 3;
//ES6 等价写法
var [a,b,c] = [1,2,3];
```
#### 提取JSON数据
```javascript
var jsonData = {
    id:42,
    status:'OK',
    data:[867,5309]
}
let { id, status, data:number } = jsonData;
console.log(id, status, number)   //42,OK,[867,5309]
```
## 数组的扩展
#### Array.from() 
`Array.from` 方法用于将两类对象转为真正的数组：类似数组的对象和可遍历对象
```javascript
//下面是一个类似数组的对象，Array.from 将它转为真正的数组。
let 
```