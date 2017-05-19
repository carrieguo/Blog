---
layout:            post
title:             "模块化编程工具 RequireJs"
date:              2017-05-17 10:10:00 +0300
tags:              Require.js
category:          JavaScript
author:            carrie
math:              true
---
1. 有效地防止命名冲突
2. 声明不同js文件之间的依赖
3. 将代码以模块化的方式组织

## 使用
1. 在html文件中引入require.js,指定入口文件main.js,它会第一个被加载require.js。由于require.js默认的文件后缀名是js，所以可以把main.js简写成main。
```html
<script src="js/require.js" data-main="ja/main"></script>
```
2. main.js
```javascript
requirejs.config({
　　　　paths: {
　　　　　　"jquery": "jquery.min",
　　　　　　"underscore": "underscore.min",
　　　　　　"backbone": "backbone.min"
　　　　}
　　});
requirejs(['jquery', 'underscore', 'backbone'], function ($, _, Backbone){
　　　　// some code here
        $('body').css('background','red');
　　});
```
3. 模块
```javascript
//math.js
//依赖jquery
define(['jquery'],function (){
　　　　var add = function (x,y){
　　　　　　return x+y;
　　　　};
       
　　　　return {
　　　　　　add: add,
          a:function(str1,str2){}
　　　　};
　　});
//main.js
requirejs(['jquery', 'math','underscore', 'backbone'], function ($, _, Backbone){
　　　　// some code here
        $('body').css('background','red');
　　});

```

#### 创建
```javascript
new Array(2);   //包含两项，每一项都是 undefined
new Array('a'); //
var a = ['a','b'];
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


