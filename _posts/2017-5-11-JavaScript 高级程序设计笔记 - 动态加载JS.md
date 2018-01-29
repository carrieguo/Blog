---
layout:            post
title:             "动态加载JS"
date:              2018-01-26 16:45:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---

### 动态加载js适用情况
1. 不确定JS是否一定在当前页面执行，需要一定条件来判断是否加载js。
2. 有不同的js,判断之后再加载需要运行的js。

### 四种不同的方式动态加载js
#### 第一种(异步加载)
1. 先创建script标签，再设置标签的type属性。
2. 设置标签的src属性。将js地址赋值给src属性。
3. 将标签插入文档。
#### 第二种(同步加载)
1. 先创建script标签，再设置标签的type属性。
2. 将js代码做成文本节点插入到script标签中
3. 将script节点插入到文档。
`缺点：ie 不支持将文本节点插入到script标签中`
#### 第三种(同步加载)
1. 先创建script标签，再设置标签的type属性。
2. 将js代码转换为字符串，然后赋值给script标签的text属性，
3. 再将script节点插入到文档。
`缺点：safair浏览器不支持`
#### 第四种 使用try catch(同步加载)
鉴于第二种和第三种方式有浏览器兼容性问题引出。
1. 先创建script标签，再设置标签的type属性。
2. 首先使用try语句尝试将代码作为文本节点插入，若报错，进入到catch语句。
3. 将js代码以字符串的形式赋值给script标签的text属性。
