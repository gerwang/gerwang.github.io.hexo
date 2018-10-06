---
title: webpack基本概念
tags: [webpack]
date: 2018-10-05 21:01:02
categories: 软工
---

一些基本概念在<https://webpack.docschina.org/concepts/>。

### 如果指定多个入口的话，输出会是怎样的呢？

答：用`output:{filename: '[name].js'}`指定每个入口各自的输出文件名。



在`webpack.config.js`中`modile.rules`中每一项的`use`是一个数组，可以对某个类型的文件按顺序使用一系列loader。

在webpack里填import的路径时，如果是基于代码所在目录的相对路径，一定要以`.`开头，否则就是从`resolve.modules`或`resolve.alias`里面开始搜索文件了。

代码中的`import`和`require`语句在经过webpack的打包处理之后变成了`__webpack_require__`语句，它们为每个模块创建了manifest信息。

使用HMR技术可以将代码中的改动快速应用到运行中的环境，从而加快开发的速度。

