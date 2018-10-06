---
title: 一些部署的配置
tags: [vue, 前端]
date: 2018-10-05 12:44:29
categories: 软工
---

### gzip资源压缩

对于支持gzip压缩的浏览器来说，gzip可以让文本资源在uglify之后在缩减50%的大小。

- [ ] 调查在Vue和egg.js中如何开启gzip压缩
- [ ] 可以看这里<https://segmentfault.com/a/1190000012571492>。

### 是否做服务端渲染？

见<https://www.jianshu.com/p/6516f2a3ce36?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation>。

我认为我们的项目至多需要一个（webpack提供的）快速的首屏渲染，或者可以直接不做SSR。

### VSCode开发配置

在VSCode上安装[vetur](https://github.com/vuejs/vetur)插件。和插件搭配的工程模板有[veturpack](https://github.com/octref/veturpack)。

### Vue测试

- [ ] 如何测试看这里<https://cn.vuejs.org/v2/cookbook/unit-testing-vue-components.html>。

### ts.d编写规范

<https://oyyd.gitbooks.io/typescript-handbook-zh/content/gitbook/classes.html>

### ES2015的新特性

`()=>`会和它的parent环境共享`arguments`变量。

在一个对象中，`handler,`是`handler:handler,`的简写。

数组和对象支持解包，如

```js
// list matching
var [a, ,b] = [1,2,3];
a === 1;
b === 3;
```

支持字面的二进制数和八进制数，如：

```js
0b111110111 === 503 // true
0o767 === 503 // true
```

