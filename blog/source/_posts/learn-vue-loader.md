---
title: 学习vue loader
tags: [vue-loader]
date: 2018-10-06 00:34:03
categories: 软工
---

我觉得，对于`.vue`文件来说，`vue-loader`才真正决定了它的运行环境。

比如`.vue`中的`<script>`支持commonJS的`require`，本质上是被`vue-loader`解析的。

在`.vue`中，最好`<script>`导出的是普通对象，就是传进`Vue.component`的那种。

在`.vue`的CSS中，可以使用`scoped`使CSS只作用于当前组件中的元素。

对于子组件的**根节点**，它的样式同时受自己和父组件的CSS影响。

`vue-cli`封装了webpack，在使用`vue-cli`时，hot-reload功能是默认开启的。

> 如果路径以 `~` 开头，其后的部分将会被看作模块依赖。这意味着你可以用该特性来引用一个 node 依赖中的资源

> 所有 `vue-cli` 创建的项目都默认配置了将 `@` 指向 `/src`

> `url-loader` 允许你有条件将文件转换为内联的 base-64 URL (当文件小于给定的阈值)，这会减少小文件的 HTTP 请求。如果文件大于该阈值，会自动的交给 `file-loader` 处理。

