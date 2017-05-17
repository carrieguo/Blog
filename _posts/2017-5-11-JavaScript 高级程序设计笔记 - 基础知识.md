---
layout:            post
title:             "基础知识"
date:              2017-05-11 14:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---

### 语句

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
    * 使用 var 操作符，定义一个变量，作用域为当前函数，基本数据类型不可改变。
    * 不使用 var 操作符，本质上是给 windows 增加了一个全局属性，对象可被改变，对象上的属性可以被删除，对象上的属性是一个无序列表。
```javascript
var message;    //局部变量，不可删除
message;        //全局变量，可被删除
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

    在JS中，所有的数据类型都可以被转换为 Boolean 型。
    
    只有以下六个转换成布尔类型时会返回false,包括`Undefined, none, 0, -0, NAN, 空字符串`，其余都会返回true，包括空对象。
    
    Tips:
    * 可以使用 `!!` 将其它数据类型转换为 Boolean 型

* `Number` 包括 `常规数字, 无穷, NAN`

    在JS中，所有的数据类型都可以被转换为 Number 型。
    
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
    
    Tips:
        * 可以在变量后加一个空字符串 `+'''` 将其它数据类型转换为 String 型。
    
#### 基本数据类型转换总结
1. Number,Boolean,String 都有一个和自己同名的函数，用来转换数据类型。
2. 函数首字母都大写
3. 参数可以是6中任何数据类型
4. 返回值是相应的同名的数据类型

### 复杂数据类型 `Object`
对象有属性，对象有方法，对象可改变
1. 内部对象
    * 错误对象
    * 常用对象 8种 `String,Number,Boolean,Object,Function,Array,Date,RegExp`
    * 内置对象 `Global,Length,Json`
2. 宿主对象 `Windows，document`
3. 自定义对象

#### 创建对象

```javascript
//使用 new 操作符后跟 Object 构造函数
var person = new Object(); //没有参数时，括号可省略
person.name = 'Carrie';
person.age = 29;

//对象字面量表示法
var person = {
    name : 'Carrie', //加入属性名含空格，可以给属性名加引号 'first name' : 'Carrie'
    age : 29
};
```

#### 访问对象 `.`,`[]`
    如果后面跟着的值是 null 或 undefined，会直接报错（null 和 undefined 没有方法），如果前面不是一个对象，会将前面的数据自动转换为对象。`.`,返回对应的值；`[]`, 先计算括号的值，再将计算的值转换为字符串，最后返回字符串对应的值，如果不存在返回 undefined。
    

#### 基本数据类型转换为 Object 类型
Object存在一个 同名函数，将参数转换为Object类型
1. Undefined Null 得到一个空对象
2. String 有好几个属性，其中有 length,按数字的索引
`String {0: "a", 1: "b", 2: "c", length: 3, [[PrimitiveValue]]: "abc"}`
3. Boolean,Number 得到一个对象，属性名称为初始值，对应的值为true
`[[PrimitiveValue]]: 123`

#### Object 类型转换为基本数据类型
1. 转换为Boolean 类型都返回 true。
```javascript
Boolean(new Boolean(false));
//将布尔值false转换为对象再转换为布尔值结果是true，
```
2. 转换为 String 类型，

    系统会先调用 toString() 方法：将对象将字符串形式表达出来
    
        * Array,将数组中的每一项用逗号分隔开，并且合并成一个字符串。
        * Function,当前函数的源代码
        * Date, 日期和时间组合的字符串
        * RegExp, 包含转义字符的正则字符串
        
    如果取不到值，再调用 valueof()：
    
        * 有原始值返回原始值
        * 无原始值，返回对象自身
        * Date对象会返回一串数字
        
3. 转换为 Number 类型
       和 String 相反，先调用 valueof()，再调用 toString()。
 
4. 如果不确定转换为 String 类型还是 Number 类型，这就变成了对象到原始值的转换。
    进行对象向原始值的转换，先调用 valueof(),可能得到的还是自身的对象（也可能得到一个原始值），再调用 toString()。
5. Date 对象转换成原始值，直接调用 toString()。
   
####比较
* 原始数据类型的比较是值的比较
* 对象的比较是引用的比较

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
* "undefind" - 如果这个值未定义
* "bollean" - 如果这个值是布尔值
* "string" - ...字符串
* "number" - ...数值
* "object" - ...对象或null
* "function" - ...函数

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

#### 优先级
属性访问`. ()` > 一元操作符 `! + - ++ --` > 乘除 `* /` > 加减 `+ -`> 比较`> <` > 相等 `== === !=` > 与 `&&` > 或`||` > 三目 `: ?` > 赋值`=` > 分隔 `,` 

```javascript
a=3;
++a==3; /*(++a)==3;*/
```
#### 结合性
优先级相同时，考虑结合性
1. 左结合 `其他`
2. 右结合 `一元操作符，三目运算，赋值运算`

```javascript
!a++; //!(a++);

x=a?b:c?d:e?f:g; // x=(a?b:(c?d:(e?f:g)));
```

#### 运算顺序
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

### 特殊的二元加操作符
    在js解释器中，进行相加操作时，如果两个操作符中有一个是字符串，另外一个也会被转换为字符串，然后再进行字符串拼接；如果两端的操作数中没有 String，但有 Undefined,Number,null,Boolean 时，会先转换为Number类型再相加。
   
1. 两边只要有一个是 String 类型，先转为 String 类型再拼接。
2. 如果两端的操作数中没有 String，有 Number 或 Boolean 类型，先转换为数字再相加。
3. 和对象相加，进行对象向原始值的转换，先调用 valueof(),得到的还是自身的对象，再调用 toString()。
4. Date 对象转换成原始值，直接调用 toString()。

```javascript
//数组调用 valueof 方法返回自身，在调用 toString 返回各项组合起来的字符串
var a = new Array(1,2);
var b = 1;
b+a; // '1'+'1,2' => '11,2'

()+(); // '' 空数组相加
 
1+{a:1};// 1[object][object]

{a:1}+1;// 1 解释器将大括号认为一段代码的结束，将+变成了一元操作符

{}+{};// NaN, 第一个空对象被认为是一段代码的结束，一元加将后面的空对象 [object][object]转换为数字

({}) + {};//[object Object][object Object]

{} + ();//0 把空数组进行数字类型转换
```

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
    
## 基础语句和分支语句
### 声明语句 `var,function`
* 在语句中定义一个变量或函数，变量或函数的作用范围仍然可以跳出此语句，在语句外仍然可以访问到此变量或函数。`JS 不存在块级作用域，只有函数作用域`
* var 定义的变量和 function 定义的函数不能用 delete 操作符删除。
* var 定义变量不设置初始值，变量的初始值为undefined
* var a=1,b,c=2;
* js解释器有一个预处理的过程，从上到下依次扫描，将 var 定义的变量的声明（仅仅是声明，不包括赋值）提前到函数体之前。将 function 定义的函数提前到代码最前面。`变量声明提前,函数定义提前`
* 匿名函数 赋值给一个变量 `var xxx = function(){};`

### if 语句
* 基础数据类型比较时最好用 === ，准确性高且提升性能，== 会多一次数据类型转换。
* if 与 else 嵌套时，就近匹配
```javascript
//实际上 else 与第二个 if 匹配
if(a==1)
    if(b==2)
        alert('123');
else
    alert('456');
```
### for-in 循环
for-in 进行对象属性的枚举
是一种精准的迭代语句，耗性能，所以一般不推荐使用，只有在列举对象属性时推荐使用。

先执行 in 后面的表达式，结果如果是 null 或 undefined 直接报错或跳过；如果是其他原始类型，转换成对象；如果是对象，直接进行下一步操作。将对象的属性取出，复制给in前面定义的变量，之后执行循环体，重复上述步骤。
1. in 前面的表达式可以是任意表达式。
2. in 前面的表达式在每次执行中都会被执行一次。
```javascript
//将对象的属性读到数组中
for(a[i++] in object);
```
3. 对基本数据类型 Number，Boolean 进行 for-in 循环，不会进行任何操作，因为他们被转换为对象只包含一个“原始值”不可被枚举。字符串转换为一个包装对象，数字对应的索引可以被枚举。
4. 只有可以被枚举的属性可以进行 for-in 循环，不可以被枚举的例如 JS 内置的 toString,valueof 方法；字符串对应的长度；原始值。
5. 对象的属性和方法继承自其它对象，进行 for-in 操作时，被继承的属性方法也会被枚举。
6. 循环体中动态增加或删除了未被枚举的属性，那么这个属性不会出现在枚举列表中。
7. 对象是一个无序的属性合集，枚举的次序按定义的顺序。
8. 对象具有整数的属性，例如整数的字符或字符串，for-in循环按照数字的顺序进行循环。

### 中断语句 `continue, break, return, throw`

