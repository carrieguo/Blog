---
layout:            post
title:             "继承"
date:              2017-05-26 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---
JS语言中虽然不存在继承，但是我们可以通过一些手段来模拟继承。

### 原型链
    b.prototype = new a;
