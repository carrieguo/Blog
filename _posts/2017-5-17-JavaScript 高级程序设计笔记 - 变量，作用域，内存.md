---
layout:            post
title:             "变量、作用域、内存"
date:              2017-05-17 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---

### 基本类型和引用类型
#### 创建变量并赋值的过程
1. 创建一个名字
2. 给其创建作用域
3. 声明提前，提升到函数开始的地方
4. 检测赋值的类型，如果赋值是基本类型,解释器会把其保存到变量中；如果赋值是引用类型,对象会保存到内存中,将内存地址保存到变量中。
变量修改
1. 基本数据类型的变量复制后，是相互独立的；引用类型的变量复制后，只是把内存的地址复制。


#### 函数参数
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

####  检测类型
1. typeof 运算符 `一元操作符`
`string number boolean undefined object function`

2. instanceof `二元操作符`
检测变量是哪种对象类型，返回值为boolean类型，对基础数据类型永远返回false。
`待检测对象 instanceof 对象类型`