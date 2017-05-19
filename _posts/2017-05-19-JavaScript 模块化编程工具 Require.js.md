---
layout:            post
title:             "模块化编程工具 Require.js"
date:              2017-05-19 10:10:00 +0300
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
