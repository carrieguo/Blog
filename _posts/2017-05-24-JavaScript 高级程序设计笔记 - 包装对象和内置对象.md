---
layout:            post
title:             "包装对象和内置对象"
date:              2017-05-17 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---
对基础数据类型也可以调用其方法
当解释器逐行扫描代码时,发现了 `.` 或 `[]`, 判断前面的变量是否为对象，如果不是，则创建和基础数据类型对应的对象。访问之后会立即删除。调用时临时创建的这种对象称为包装对象
## 包装对象
```javascript
var a = 'abcd';
a.length = 10;  //创建了一次包装对象，立即销毁
console.log(a.length);  //4。又创建了一次包装对象
```
所有的包装对象都有对象的基本方法 toString valueof

#### Boolean
* valueof() 返回本身
* toString() 返回本身的字符串

#### Number
* valueof()
* toString()
* toFixed() 保留小数点后的位数
* toPrecision() 保留数字的位数

#### String
* charAt(3)  获取第四位
* charCodeAt(3) 获取第四位的字符编码
* indexOf('a') 查找字符a位置
* lastIndexOf('a') 查找字符a位置,从末尾查找
* split(',')   基于指定的分隔符将一个字符串分割成多个字符串，存放到数组中

基于子字符串创建新字符串。

* slice('3','-2')    
* substring('3','-2') //相等于 substring('0','3')
* substr() //类似于substring,第二个参数是截取几个字符串的意思
1. 当第二个参数小于第一个参数时，substring 会调换两个参数位置；slice会返回undefined
2. 对于负数参数，slice会将负数视为倒数，substring会将负数视为0。

* toLowcase()
* toUppercase()
* trim()
* trimLight()
* trimRight()

ES6
startWith()
endWith()
includes('a') 是否包含字符串a
repeat('abc') 重复

#### 常见问题
字符串反转
```javascript
split() //分割成数组
reverse()   //数组反转
join()  //数组组合成字符串
```
输出今天星期几
```javascript
array.chatAt(new Date().getDay())
```

## 内置对象
JS解释器中已定义好的对象。
不需要new操作符创建的对象（无需实例化），没有构造方法。
1. Global 全局对象 访问不到，只是一个概念，表示全局环境

    * 处理数字`isFinite,isNaN(判断是否能转换为数字),parseint,parsefloat`
    
    * 处理URL `encodeURI,encodeURIComponent`
    
    * eval() 改写解释器，性能不好
        * 转Json结构；
        * 动态声明局部变量；
        * 代码压缩；
        * 接受字符串作为参数，将字符串运行
    严格模式下外部访问不到eval()中创建的任何变量或函数；存在安全隐患
    
2. Math
    ceil 向上取整
    random 生成0到1的一个随机数，不会出现0或1
```javascript
//生成随机字符串
Math.random().toString(36);
```
3. Json
    字符串和json互转