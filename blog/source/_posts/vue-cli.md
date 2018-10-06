---
title: 学习使用vue-cli
tags: [vue]
date: 2018-10-05 16:01:43
categories: 软工
---

在安装了`@vue-cli`和`@vue/cli-service-global`之后，用`vue serve`可以打开一个临时服务器查看`.vue`或者`.js`文件的效果。

创建项目时如果用`vue ui`可以打开浏览器，用图形化界面的形式设置创建的选项。

> `cache-loader` 会默认为 Vue/Babel/TypeScript 编译开启。文件会缓存在 `node_modules/.cache` 中——如果你遇到了编译方面的问题，记得先删掉缓存目录之后再试试看。

- [ ] 这里有配置`vue-cli`的参考<https://cli.vuejs.org/zh/config/>。

> **如果该依赖交付 ES5 代码，但使用了 ES6+ 特性且没有显式地列出需要的 polyfill (例如 Vuetify)：请使用 useBuiltIns: 'entry' 然后在入口文件添加 import '@babel/polyfill'。这会根据 browserslist 目标导入所有** polyfill，这样你就不用再担心依赖的 polyfill 问题了，但是因为包含了一些没有用到的 polyfill 所以最终的包大小可能会增加。

在`vue-cli-service build --modern`中，适合现代浏览器的ES module和兼容老浏览器的代码被同时提供给浏览器，浏览器根据自己的情况来下载和忽略。注意一点：

> <script type="module"> 需要配合始终开启的 CORS 进行加载。这意味着你的服务器必须返回诸如 Access-Control-Allow-Origin: * 的有效的 CORS 头。如果你想要通过认证来获取脚本，可使将 crossorigin 选项设置为 use-credentials。

可以用项目根目录下的`.env`文件设定在某种运行模式下的环境变量值。