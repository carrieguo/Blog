---
layout:            post
title:             "函数"
date:              2017-05-17 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---
函数定义使用过程中，传递参数

## 函数创建
1. 函数声明语句
    
    函数声明提前，可以先执行后定义；不建议在语句块中使用函数声明语句。
2. 函数表达式语句 `匿名函数`

    只提前声明的变量，先执行后定义会报错；可以在语句块中使用函数表达式语句。
    所有的函数都是匿名函数。
    

```javascript
//1. 函数声明语句
function name(){}
//2. 函数表达式语句
var a = function() {};
//匿名函数赋值，对象创建方法，回调函数
```
```javascript
//不建议在语句块中使用函数声明语句。
if(aa){
    function aaa(){}
}
else {
    function aaa(){}
}
```
3. 命名函数表达式
    方便调试
```javascript
var a = function name() {}

function ok(){}
ok.name; //ok

a.name; //name

```
4. ES6 箭头函数

    没有自己的上下文环境。
    是一个匿名函数
    name属性是一个空字符串
    没有argument属性
```javascript
()=>{}

//以下两种写法相同
sum(function(item){return item===0});
sum(item=>item===0);
```
5. ES6 函数生成器
    一种闭包。
    可以对状态进行保存。
    yield 类似 return, 可存在多个，返回迭代器对象
```javascript
function *
```

6. 函数构造器 `不推荐使用`
```javascript
new funtion('sum1','sum2','return sum1+sum2');
//调用了eval，耗性能，不安全；污染全局环境；
```

## 将函数作为参数

回调函数 `文件读取，数据库操作` 异步操作
```javascript

```
操作函数

## 将函数作为返回值

## 闭包
闭包中的变量会一直存在内存中，外部无法访问
```javascript
function a(){
    var i=0;
    return function() {
        alert(i++);    
    }
}
var b = a();
b(); //0，1，2...
```

随机生成不重复 id
```javascript
var a = 'abc';
var b = 0;
function getID() {
    return a+b++;
}
//利用闭包，外部无法访问变量a,b
var getID =(function(){
    var a = 'abc';
    var b = 0;
    return function() {
      return a+b++;
    }
})();
```

如果代码在全局环境下运行，会生成一个列表——作用域链，只包含一个活动对象，运行函数时会创建一个函数作用域链，一个指向全局活动对象，另一个

```javascript
var a;
function b() {
  function c(){}
}
```