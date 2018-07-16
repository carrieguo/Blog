---
layout:            post
title:             "react + webpack 环境搭建"
date:              2018-07-16 10:10:00 +0300
tags:              JavaScript 笔记
category:          JavaScript
author:            carrie
math:              true
---
## webpack + react 环境搭建
1. 全局安装webpack, babel, webpack-dev-server
npm install babel webpack webpack-dev-server -g
2. 在项目中安装react,react-dom
npm install react react-dom --save
3. 安装babel转换器（以下插件可能有的已经过时，现在可能已经换成了babel-preset-env）
npm install babel-loader babel-core babel-preset-react babel-preset-latest --save
4. 创建项目文件，main.js 即项目入口文件，App.js 即 React 组件主文件
touch index.html App.js main.js webpack.config.js
5. 配置webpack，编辑webpack.config.js
```javascript
module.exports = {
    entry: './main.js', // 入口文件路径
    output: {
        path: '/',
        filename: 'index.js'
    },
    devServer: {
        inline: true,
        port: 3333,
	open: true
    },
    module: {
        loaders: [
            {
                test: /\.js$/, // babel 转换为兼容性的 js
                exclude: /node_modules/,
                loader: 'babel-loader',
                query: {
                    presets: ['react', 'latest']
                }
            }
        ]
    }
}
```
