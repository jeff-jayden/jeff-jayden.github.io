---
title: babel
comments: true
date: 2025-01-03 16:25:52
tags:
---

babel 学习记录

babel-plugin-transform-runtime插件的能力：为了方便使用 babel-runtime，解决手动 require 的苦恼 

@babel/preset-env也是一个预设，包含很多插件

babel-runtime 是 @babel/polyfill 的后者 是垫片，是各种插件集合，主要为了能转换新的 API 例如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise 等全局对象
babel-runtime 更像是一种按需加载的实现

工作原理：解析，转换，生成


babel中有很多插件，也有预设，包含一系列插件，即presets,还有 core 包，parser 包，traverse,generator

eslint 也依赖 babel

参考文章

https://cloud.tencent.com/developer/article/2100524
https://blog.csdn.net/weixin_44019380/article/details/132684899
https://juejin.cn/post/6844903956905197576