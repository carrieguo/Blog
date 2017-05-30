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
#### Class 的继承
Class之间可以通过extends关键字实现继承
```javascript
class ColorPoint extends Point{
    constructor(x,y,color){
        super(x,y);     //  调用父类的constructor(x,y)
        this.color = color;
    }
    toString(){
        return this.color + ' '+ super.toString(); //调用父类的toString（）
    }
}
```
## Module
ES6 的模块自动采用严格模式，不管有没有在模块头部加上 "use strict"。

严格模式主要有以下限制
* 变量必须声明后再使用。
* 函数的参数不能有同名属性，否则报错。
* 不能使用with语句。
* 不能对只读属性赋值，否则报错。
* 不能删除不可删除的属性，否则报错。
* arguments不会自动反映函数参数的变化。
* 禁止this指向全局对象。
* 增加了保留字（比如 protected、static 和 interface）
#### export 命令
模块功能主要由两个命令构成：export和import。export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。
```javascript
//ca.js
export var year = 1958;
export var name = 'carrie';
export {name,year};
export function multiply(x,y){
    return x*y;
};
```
#### import　命令
```javascript
import {name,year} from './ca.js';
```
## Generator 函数
    Generator 函数是ES6提供的一种异步编程解决方案。
    从语法上，首先可以把它理解成一个状态机，封装了多个内部状态。
    执行Generator函数会返回一个遍历器对象。也就是说Generator函数还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历Generator函数内部的每一个状态。
```javascript
function* hello() {
    yield 'hello';
    yield 'world';
    return 'end';
}
var hw = hello();
hw.next()
//{value:'hello',done:false}
hw.next()
//{value:'world',done:false}
hw.next()
//{value:'end',done:true}
hw.next()
//{value:undefined,done:false}
```
需要调用遍历器对象的next方法，是的指针移向下一个状态。Generator函数是分段执行的，yield语句是暂停执行的标记，而next方法可以恢复执行。
#### next方法的参数
yield语句本身没有返回值，或者说总是返回undefined。next方法可以带一个参数，该参数会被当作上一条yield语句的返回值
```javascript
function* f() {
    for(var i=0;true;i++){
        var reset = yield i;
        if(reset){i=-1}
    }
}
var g=f();
g.next()    //{value:0,done:false}
g.next()    //{value:1,done:false}
g.next(true)    //{value:0,done:false}
```
    上面定义了一个可以无限运行的Generator函数f。当next方法带一个参数true时，当前的变量reset就被重置为这个参数（即true）,因此i=-1,下一轮循环就从-1开始递增。
    这样，可以在Generator函数运行的不同阶段，从外向内注入不同的值，从而控制函数行为。
## Promise 对象
Promise是一个对象，用来传递异步操作的消息。
ES6规定，Promise对象是一个构造函数，用来生成Promise实例。
```javascript
//用Promise对象实现AJAX操作
var getJSON =function(url) {
    var promise = new Promise(function(resolve,reject){
        var client = new XMLHttpRequest();
        client.open("GET",url);
        client.onreadystatechange = handler;
        client.responseType = 'json';
        client.setRequestHeader('Accept','application/json');
        client.send();
        
        function hander(){
            if (this.readyState !==4){
                return;
            }
            if(this.status===200){
                resolve(this.response);
            }else{
                reject(new Error(this.statusText));
            }
        };
    });
    return promise;
};
getJSON('/posts.json').then(function(json){
    console.log(json);
},function(error) {
    console.error(error);
});
```
    Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由JavaScript引擎提供，不用自己部署。
    resolve 函数的作用是，将 Promise 对象的装态从Pending 变为Resolved,在异步操作成功时调用，并将异步操作的结果作为参数传递出去；reject函数的作用是，将Promise对象的状态从Pending变为Rejected,在异步操作失败时调用，并将异步操作报出的错误作为参数传递出去。
    Promise实例生成以后，可以用then方法分别指定Resolved状态和Rejected状态的回调函数。
    then方法接受两个回调函数作为参数。第一个回调函数是 Promise 对象的状态变为Resolved 时调用，第二个回调函数是Promise对象的状态变为Rejected时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受 Promise 对象传出的值作为参数。

#### Promise.all()
将多个 Promise 实例包装成一个新的 Promise 实例。
```javascript
var p = Promise.all([p1,p2,p3]);
```
上面的代码中，Promise.all方法接受一个数组最为参数，p1、p2、p3都是Promise对象的实例；如果不是，就会先调用Promise.resolve方法，将参数转为Promise实例，再进一步处理。
p 的状态由p1、p2、p3决定
1. 如果p1、p2、p3状态全成功，就会将p1、p2、p3的返回值组成一个数组，传递给p的回调函数
2. 只要p1、p2、p3中有一个被Rejected,p的状态就变成Rejected,此时第一个被Rejected的实例的返回值会传递给P的回调函数。

#### Promise.race()
将多个 Promise 实例包装成一个新的 Promise 实例。
```javascript
var p = Promise.race([p1,p2,p3]);
```
上面的代码中，只要p1、p2、p3中有一个实例率先改变状态,p的状态就跟着改变。那个率先改变的Promise实例的返回值，就传递给p的回调函数。
#### Promise.resolve()
将现有对象转为Promise对象。且其对象是Resolved。
#### Promise.reject()
返回一个新的Promise实例，状态为Rejected。