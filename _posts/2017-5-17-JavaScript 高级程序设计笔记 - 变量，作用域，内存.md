---
layout:            post
title:             "变量、作用域、内存"
date:              2017-05-17 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
intensedebate_comments: true
---

## 变量
#### 创建变量并赋值的过程
1. 创建一个名字
2. 给其创建作用域
3. 声明提前，提升到函数最顶端 `如果有同名函数，函数声明会覆盖变量声明，并提前`
4. 检测赋值的类型，如果赋值是基本类型,解释器会把其保存到变量中；如果赋值是引用类型,对象会保存到内存中,将内存地址保存到变量中。

变量修改
1. 基本数据类型的变量复制后，是相互独立的；引用类型的变量复制后，只是把内存的地址复制。

#### 函数参数
ECMAScript中所有的函数的参数都是按值传递的。
1. js 的解释器不关心函数参数的数据类型,也不关心参数的数量。
2. 传递的参数多与函数定义的参数，多余的参数不参与运算；若参数少，则默认缺省的参数为 undefined。
3. 函数的参数其实是一个数组对象。

```javascript
function add(){
    return arguments[0]+arguments[1]; //函数的数组对象
}

//等价于
function add(a,b){
    return a+b;
}
```
4. 在函数中所有参数的传递，都是按值来传递。引用的传递实际上是将这个值在内存中的地址复制给一个局部变量。
如果参数是引用类型，传递的参数作用范围在函数外；复制的参数是一个局部变量，作用范围在函数内部。
```javascript
function setName(obj) {
  obj.name = 'Carrie';
  obj = new Object();
  obj.name = 'Bill';
}
var person = new Object();
setName(person);
alert(person.name); //'Carrie'
//实际上，挡在函数内部重写obj时，这个变量引用的就是一个局部对象了。而这个局部对象会在函数执行完毕后立即被销毁。
```
####  检测类型
1. typeof 运算符 `一元操作符`
`string number boolean undefined object function`

2. instanceof `二元操作符`
检测变量是哪种对象类型，返回值为boolean类型，对基础数据类型永远返回false。
使用方法 `待检测对象 instanceof 对象类型`

## 作用域
概述：计算机对值进行保存，读取的一套规则。JS 基于函数创建作用域。

js程序运行时经历两个步骤，先编译，后执行。

    编译阶段先进行词法语法分析，后生成代码。
    在词法语法分析阶段，创建作用域，保存了变量和函数，限定了它们的使用范围。

代码运行时，创建作用域链，连接了之前创建的作用域，建立对之前保存的变量函数的访问规则，当对变量进行操作时，首先在当前作用域下查找变量，如果没找到就会顺着作用域链向上查找。
    
#### 作用域

js 解释器编译时，会创建作用域并生成一段代码，并使用大量技巧来提升编译速度。
1. 编译过程 `var a=123;`

    解释器创建一个全局作用域，进入词法语法分析阶段，对声明语句 `var a`，解释器如果发现当前作用存在a 就忽略声明，如果没有，则创建一个变量a,并分配存储空间。
    假如声明是一个函数，还需要给函数新建一个作用域。假如同一作用域下，有两个重名的函数和变量，在当前作用域下，函数会覆盖变量。
    作用域可以嵌套，但是各自不能重叠。
    
2. `with, eval` 可以自己设置作用域,但是它为了正确性，编译时不进行优化，破坏了解释器编译时的规则，会降低编译速度，所以不建议使用。

#### 作用域链
编译过程生成的代码，在运行阶段会创建作用域链。作用域按照一定的规则对函数或变量进行`存储`。作用域链是对作用域中的变量或函数进行`访问`。

代码运行时，每进入到一个函数都会创建作用域链，连接和自己上层的作用域，查找变量时，从当前到上层查找，一直到全局作用域。如果一直没找到，会在全局创建一个同名属性，

js 不存在块级作用域，只有函数作用域。（ES5）

#### 垃圾回收
为了保证内存合理使用,JS建立了垃圾回收机制，每隔一段时间运行一次。

手动管理 : 将变量设置为null,系统会自动回收。
