---
title: 解决vue-cli 在多页面应用中HMR特别慢的问题
categories: vue
tags: webpack
keywords: [vue-cli, webpack, HMR]
description: 解决多页面应用在热更新的时候卡住或者构建特别慢，甚至有时候会导致js内存溢出，直接退出编译的问题
---
# 解决vue-cli 在多页面应用中HMR特别慢的问题

作为webpack中的第一大插件html-webpack-plugin,大家应该或多或少的使用过，这个插件会根据你的模板代码，通过不同的模板引擎构建出对应的html,ejs甚至ftl文件，在标准的SPA中，该插件性能不会性能瓶颈，但是如果你使用的是多页面，该插件的构建速度绝对是地狱级别的，
如，我只是简单修改了一个vue文件的一个文案，在阶段居然花费了16s，这大大减慢了开发效率，感受不到HMR的优势。
我司有个用vue-cli搭建的多页面应用，随着不断的迭代，项目越来越大，导致开发的时候热更新特别慢，有时候能到到十六七秒，这样的很是影响开发效率，有时候还导致js内存溢出，直接退出编译。一脸蒙蔽的我赶紧掏出我的必杀技，百度、google一顿搜索，终于找到了我想要的答案，原来是html-webpack-plugin会对每个入口文件都执行一遍emit中所有的代码，然后发现已经有人帮我们造好了轮子

```
html-webpack-plugin-for-multihtml
```
github地址

```
https://github.com/daifee/html-webpack-plugin-for-multihtml
```
首先安装插件

```
npm i html-webpack-plugin-for-multihtml --save-dev
```
修改配置代码

```
const HtmlWebpackPlugin = require('html-webpack-plugin-for-multihtml');
// 省略其他代码

plugins:[
  new HtmlWebpackPlugin({
    template: filePath,
    filename: `${filename}.html`,
    chunks: ['manifest', 'vendor', filename],
    inject: true,
    multihtmlCache: true  // 增加该配置
  })
]
```
升级完成后HMR的速度会得到很大的提升


