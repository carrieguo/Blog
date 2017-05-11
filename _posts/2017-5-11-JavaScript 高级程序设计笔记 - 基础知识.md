---
layout:            post
title:             "基础知识"
date:              2017-05-11 14:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---
###语句

省略分号时，解释器把能合并的代码和当前行合并，但是`++`和`--`除外，它们和下一行合并。
```javascript
a=1
b=2
a
++
b
//a=1,b=3
```
### 变量

```javascript
var message; //局部变量
message;    //全局变量
```

## 数据类型 `5+1种`

### 简单数据类型（基本数据类型,原始数据类型）
* `Undefined` 

    不是一个关键字；本质上是 windows 的一个属性，typeof 返回 Undefined，数字类型转换返回 NAN。
* `Null` 

    是一个关键字；本质上是一个空对象,typeof 返回 Object, 数字类型转换返回0。
    
    null 和 Undefined 都不具备任何方法。
    
    Tips:
    * 定义时用null来初始化
    * 判断是否为空，使用==null；
    * 检测值是 null 还是 undefined，才使用===Undefined。windows属性有很多，用Undefined做判断时很消耗性能。
    
    当我们创建对象，并给属性赋值时，检测是否为空，使用==null；检测值是否存在，使用===Undefined。

* `Boolean, Bool`

    在JS中，所有的数据类型都可以被转换为 Boolean 型，只有以下六个转换成布尔类型时会返回false,包括`Undefined, none, 0, -0, NAN, 空字符串`，其余都会返回true，包括空对象。
    
    Tips:
    * 可以使用 `!!` 将其它数据类型转换为 Boolean 型

* `Number` 包括 `常规数字, 无穷, NAN`

    解释器将 `.1` 解释为 `0.1` ；将 `1.` 自动转换为整数 `1` 。
    
    注意：小数 0.1+0.2 不等于 0.3， 因为小数的二进制表示可能是一个无穷数，操作小数时可以先转换为整数。
    
    NAN 是一个特殊的 Number 类型，不等于其本身。
    `0/0 返回 NAN; 其他数值/0 返回无穷。`

    * isNaN() 判断是否能将参数转换为数值。
    * Number() 将所有的类型转换为数字。
    * parseInt()
    * parseFloat()
    
* `String` 由Unicode编码
    * .length属性 返回字符串长度
    * 当遇到`.`时，解释器对非对象类型的数据先转换为对象，再执行。在所有的数据类型中，只有null和Undefined不能转换为对象。
    toString() 方法：转换为字符串。
#### 数据类型转换总结
1. Number,Boolean,String 都有一个和自己同名的函数，用来转换数据类型。
2. 函数首字母都大写
3. 参数可以是6中任何数据类型
4. 返回值是相应的同名的数据类型

### 复杂数据类型 `Object`
对象有属性，对象有方法，对象可改变
1. 内部对象
    * 错误对象
    * 常用对象 8种 `String,Number,Boolean,Object,Function,Array,Date,Ext`
    * 内置对象 `Global,Length,Json`
2. 宿主对象 `Windows`
3. 自定义对象

```javascript
var a ='abcd';
a.length =4;
console.log(a.length); //undefined
a.toUpperCase();
console.log(a); //abcd
/*5种基本数据类型都不可变，上面变量 a 调用 .length 方法时，会先把 a 转换成 Object, 调用完毕立即销毁，所以我们得到的结果是 undefined,调用 toUpperCase 方法时也是一样。*/
```
### typeof 操作符
typeof 返回值都是字符串，使用时可带括号，也可省略。
"undefind" - 如果这个值未定义
"bollean" - 如果这个值是布尔值
"string" - ...字符串
"number" - ...数值
"object" - ...对象或null
"function" - ...函数

## 表达式
表达式和操作符构成了JS。
### 表达式的分类
1. 原始表达式 `常量，变量，直接量，关键字`
    * 常量一般全大写命名，不可改变
2. 初始化表达式，
    例如初始化对象，初始化数组。
3. 函数定义表达式
4. 函数调用表达式
5. 属性访问表达式
6. 对象创建表达式，new xxx

## 操作符
### 一元操作符
只操作一个数的操作符称为一元操作符。

1. 一元加 `+`，转换成数字
 * 对数字类型进行一元加操作，最后得到数字本身
 * 对布尔类型，对true进行一元加操作，会先将true转换为数字类型，得到1再进行一元加操作，最终得到1；false返回0。
 * 对none,返回0
 * 对Undefined,返回NAN
 * 对String,对用纯数字表示的String，返回对应的数字；其余的返回NAN。
 * 对Object,先调用valueof(),再调用toString(),可能返回数字，也可能返回NAN[*1查]
2. 一元减 `-`,先转换成数字，再取负值
3. 递增操作 `++`
4. 递减操作 `--`
5. 按位非 `~`
6. 逻辑非 `!` ,先转换为布尔类型再取非。
 
7. typeof 返回操作数的数据类型,后面可带括号，也可省略
8. void 会返回Undefined
9. delete

## 运算顺序
先考虑优先级，再结合性，后运算顺序

### 优先级
属性访问`. ()` > 一元操作符 `! + - ++ --` > 乘除 `* /` > 加减 `+ -`> 比较`> <` > 相等 `== === !=` > 与 `&&` > 或`||` > 三目 `: ?` > 赋值`=` > 分隔 `,` 

```javascript
a=3;
++a==3; /*(++a)==3;*/
```
### 结合性
优先级相同时，考虑结合性
1. 左结合 `其他`
2. 右结合 `一元操作符，三目运算，赋值运算`

```javascript
!a++; //!(a++);

x=a?b:c?d:e?f:g; // x=(a?b:(c?d:(e?f:g)));
```

###运算顺序
遵循从左往右的运算顺序

```javascript
a=1;
b=a+++a; //b=(a++)+a; 
//b=3,a=2 
```
## 逻辑运算详解
`&&` 与 `||`

```javascript
a=1;
b=2;
a==1&&b==3  //判断
a==1&&b=3   //短路写法，前面是判断，后面是执行
a==2||b=3   //判断第一个表达式为false时，才赋值
```

## 加减乘除和比较赋值
1. 先转换为数字，再进行运算
2. NAN是一个特殊的数据类型，不等于任何值，包括自己。

### 特殊的加操作
1. 如果操作符两边都是Number或Boolean类型，先转换为数字再相加。
2. 两边只要有一个是String类型，先转为String类型再拼接。

### 等号
1. === 
  *  先判断类型是否相同
  * 再判断相等，字符串判断编码，对象判断引用，none和undefined只和自身相等，NAN不等于任何值
2. ==
    * undefined 和 none 相等
    * 1 和 '1' 相等，字符串和数字，先把String转换number再判断。
    * 对象判断引用是否相等
3. = 赋值运算，二元操作符
    * number += 10; 和 number = number+10; 在性能和效果上完全一致。