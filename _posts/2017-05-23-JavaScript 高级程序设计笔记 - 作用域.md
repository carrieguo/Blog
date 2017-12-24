---
layout:            post
title:             "作用域"
date:              2017-05-23 19:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---
作用域是指JS中对数据进行存取的规则。
作用域之间可以相互嵌套，但不会相互重叠。外部作用域不能访问内部作用域，但内部作用域可以访问其外部作用域。

JS只提供了函数作用域，不支持块级作用域。我们可以通过IIFE模拟块级作用域。
函数作用域：每创建一个函数，会在当前作用域下创建一个新的作用域。
外部作用域：函数外部
#### 立即执行函数表达式(IIFE)
1. 解决了作用域下命名污染的问题。
2. 提升性能，方便在小作用域内查找。
3. 有利于压缩。可以将传递的参数用简单字符代替。
4. 解决命名冲突，假设自己定义了一个$，又使用了jquery框架，我们可以windows.jquery作为实参传递给匿名函数。

    修复bug,undefined可以赋值。假如不小心给undefined赋值了，我们可以用function(undefined){}()解决。
6. 保存闭包状态。作用域链机制使得比闭包只能取得包含函数中任何变量的最后一个值。
```javascript
var data = [];

for (var i = 0; i<=3; i++) {
  data[i] = function () {
    console.log(i);
  };
}

data[0]();  //3
data[1]();  //3
data[2]();  //3

/*
当执行到 data[0] 函数之前，此时全局上下文的 VO 为：

globalContext = {
    VO: {
        data: [...],
        i: 3
    }
}

当执行 data[0] 函数的时候，data[0] 函数的作用域链为：

data[0]Context = {
    Scope: [AO, globalContext.VO]
}

data[0]Context 的 AO 并没有 i 值，所以会从 globalContext.VO 中查找，i 为 3，所以打印的结果就是 3。
*/

//让我们改成闭包看看：

var data = [];

for (var i = 0; i<= 3; i++) {
  data[i] = (function (i) {
        return function(){
            console.log(i);
        }
  })(i);
}

data[0]();
data[1]();
data[2]();

/*当执行到 data[0] 函数之前，此时全局上下文的 VO 为：

globalContext = {
    VO: {
        data: [...],
        i: 3
    }
}

跟没改之前一模一样。

当执行 data[0] 函数的时候，data[0] 函数的作用域链发生了改变：

data[0]Context = {
    Scope: [AO, 匿名函数Context.AO globalContext.VO]
}

匿名函数执行上下文的AO为：

匿名函数Context = {
    AO: {
        arguments: {
            0: 1,
            length: 1
        },
        i: 0
    }
}

data[0]Context 的 AO 并没有 i 值，所以会沿着作用域链从匿名函数 Context.AO 中查找，这时候就会找 i 为 0，找到了就不会往 globalContext.VO 中查找了，即使 globalContext.VO 也有 i 的值(值为3)，所以打印的结果就是0。
*/
```
7. UMD通用模块规范
```javascript
~function(a){
    a;
}(function(){});
```
```javascript
//定义一个匿名函数并执行。为了防止解释器将{}解析为语句块的结束，单独执行();报错，所以将函数声明语句用小括号括起来，转换为函数表达式。
//其实区分函数声明语句和函数表达式的关键是看它们是否以开头function开头，利用这个方法，我们可以用多种方式将函数声明语句转换为函数表达式，来直接运行匿名函数，如下：
(function a(){})();
(function a(){}());
a = function(){}();
true && function(){}();
false || function(){}();
0,function(){}();
//使用一元运算符，有一次数字类型的转换，性能偏低。
+function(){}();
-function(){}();
//性能比一元加一元减好一些
~function(){}();
!function(){}();    //bootstrap用了这种方式
new function() {};  //不推荐使用，带来歧义
```

使用 IIFE 时，最好在前面加一个分号 `;` ,避免压缩代码后带来的不必要问题。

此外，在编译过程中，有三个特殊的语句，会通过欺骗词法分析来创建和修改作用域。
其中`with`和`catch`可以创建块级作用域，但是性能不好

1. with{}  块级作用域

2. try catch{} 块级作用域

3. eval()

此外ES6 的let声明的变量，语句创建的是块级作用域
```javascript
for (var i ;i<len;i++){}    //在for循环外可以访问到 i
for (let i ;i<len;i++){}    //在for循环外不能访问到 i
```
