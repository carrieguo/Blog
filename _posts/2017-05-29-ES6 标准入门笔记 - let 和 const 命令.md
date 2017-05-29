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
let arrayLike = {
    '0' : 'a',
    '1' : 'b',
    '2' : 'c',
    length : 3
};
//ES5 的写法
var arr1 = [].slice.call(arrayLike);    //['a','b','c']

//ES6 的写法
let arr2 = Array.from(arrayLike);       //['a','b','c']
//ES6 扩展运算符
let arr3 = [...arrayLike];
```
## for...of 循环
    JavaScript 原有的for...in循环，只能获得对象的键名，不能直接获取键值。ES6 提供for...of循环，允许遍历获得键值。
```javascript
var arr = ['a','b','c','d'];
for(let a in arr){
    console.log(a); //0 1 2 3
}
for(let a of arr){
    console.log(a); //a b c d
}
/*上面的代码表明，for...in循环读取键名，for...of循环读取键值。*/
```
## Map
    JavaScript 的对象（Object）本质上是键值对的集合，但是只能用字符串作为键。这给它的使用带来了极大的限制。
    为了解决这个问题，ES6 提供了Map数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键
```javascript
var map = new Map();
map.set('a','apple');
map.set('b','banana');
map.set('c','orange');
map.set('d','pear');

for(var [key,value] of map){
    console.log(key,value);
}
/*
a apple
b banana
c orange
d pear
*/
```
## 函数
#### 函数参数的默认值
ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。
```javascript
function log(x,y = 'world') {
  console.log(x,y);
}
log('Hello')    //Hello World
log('Hello','China')    //Hello China
log('Hello','')    //Hello 
```
#### 箭头函数
ES6 允许使用“箭头”（=>）定义函数。
```javascript
var f = v => v;
//等同于
var f = function(v){
    return v;
}
```
使用注意点：
1. 函数体内的this对象就是定义是所在的对象，而不是使用时所在的对象。
2. 不可以当作构造函数。也就是说，不可以使用new命令，否则会抛出一个错误。
3. 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用rest参数代替。

## 对象
#### 对象的简洁表示
```javascript
var Person = {
    name : 'Carrie',
    //等同于birth:birth
    birth,
    //等同于hello:function()...
    hello(){ console.log(this.name);}
};
```
## Class
#### Class 基本语法
JavaScript 语言的传统方法是通过构造函数定义并生成新对象
```javascript
function Point(x,y){
    this.x = x;
    this.y = y;
}
Point.prototype.toString = function() {
  return '(' + this.x + ',' + this.y +')';
}
```
ES6 提供了更接近传统语言的写法，引入了Class(类)这个概念作为对象的模板。通过class关键字可以定义类。基本上ES6中的class可以看作只是一个语法糖，它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法，新的class写法只是让对象原型的写法更加清晰，更像面向对象编程的语法而已。
```javascript
class Point{
    constructor(x,y){
        this.x = x;
        this.y = y;
    }
    toString(){
        return '(' + this.x + ',' + this.y +')';
    }
}
```