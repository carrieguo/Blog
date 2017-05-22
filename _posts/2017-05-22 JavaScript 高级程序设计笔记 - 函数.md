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
2. 函数表达式语句

    只提前声明的变量，先执行后定义会报错；可以在语句块中使用函数表达式语句
```javascript
//1. 函数声明语句
function name(){}
//2. 函数表达式语句
var a = function() {}
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



