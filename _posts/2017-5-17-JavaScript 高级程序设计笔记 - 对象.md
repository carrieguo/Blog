---
layout:            post
title:             "数组对象，时间对象"
date:              2017-05-17 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---
各种属性及其对应值组成的无序合集。

## 数组对象
#### 创建
```javascript
new Array(2);   //包含两项，每一项都是 undefined
Array.apply(null,{length:2});//上面的写法无法使用for循环遍历，可采用这种写法

new Array('a'); //
var a = ['a','b'];  //Arr

```
#### 读取
```javascript
a[0];
```

#### 删除操作
```javascript
delete a[0];
```

#### 检测数组
```javascript
if (list instanceof Array){} 

if(list.constructor == Array){}
//以上两种方法，在多个 iframe 下可能会出错

if(Array.isArray(list)){}   //es5
//最佳方式
Object.prototype.toString.call(o) === '[object Array]';
```

#### 方法
##### 栈方法，队列方法
```javascript
var list = new Array();
var length = list.push('a','b');    //推入两项并返回长度
list.pop();

list = list.shift();  //取出并返回第一项
length = list.unshift('a');    //推入，并返回长度
```
##### 重排序
```javascript
list.reverse(); //反转顺序
list.sort();    //先转换为字符串再排序
list.sort(function(a,b){
    return a-b;
});    //sort 是一种冒泡排序
```
sort()方法需要多次计算

##### 操作方法
concat()    合并多个数组
slice()     返回起始和结束位置之间的项，不会影响原始数组
splice()    返回一个新数组。
1. 删除。第一个参数为删除的第一项的位置，第二个参数为要删除的项数。
2. 插入和替换。三个参数:起始位置、0（要删除的项数）、要插入的项。

#### 伪数组
1. DOM节点
2. 函数参数argument
```javascript
//把伪数组转换为真实数组
Array.prototype.slice.call(o);
```

#### 数组去重

1. 建立数组，对老数组进行循环，如果取出的值在新数组中不存在则push。
2. 建立新数组，将老数组进行循环，比较当前项和之后的各项，发现重复则跳过，不重复则push到新数组。
3. 使用嵌套循环，在老数组中进行删除
4. 借助对象属性不能重复
5. 先排序，删除重复项

ES5
1. indexof(i,i+1)

ES6

1. set 对象里的每个成员都不重复

Area.from(new set(array));

2. 拓展运算符

...new set(array);

## 时间对象
JS 支持两种时间
* GMT 时间 格林威治标准时间 （有误差）
* UTC 时间 平均时间 （更精准）

1970年1月1日0点开始到当前的毫秒数

```javascript
new Date('2016,10,10');//月份从0开始
new Date('May 25,2016 13:20')//解析字符串
```
#### 继承方法
toString()
toLocalString()

valueof() 返回日期的毫秒表示

#### 格式化方法

toUTCString() //设置cookies过期时间

把一个时间对象转换为毫秒数有以下方法
```
valueof()   //快，推荐

getTime()   //快，推荐

Number()    //慢

+           //慢，推荐
```
