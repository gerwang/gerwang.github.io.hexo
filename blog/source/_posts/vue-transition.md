---
title: Vue的动画过渡和Mixin
tags: [vue]
date: 2018-10-04 15:59:21
categories: 软工
---

用`<transition name="a">`将需要过渡动画的元素包起来，在CSS里按照`a-enter-active`、`a-leave-active` 的格式命名。

用`appear`系列可以设置元素初次渲染时的动画行为。

即使确实是同一个元素，如果动态修改`key`，也会让Vue认为是不同的元素。

用`mode="out-in"`先删除旧元素，再让新元素进入。

用`<transition-group>`实现`v-for`的动画效果。用`tag`指定`<transition-group>`的标签名，另外在使用`<transition-group>`时不能使用`out-in`之类的。

在`render:`处的`function(createElement)`接受函数`createElement`作为第一参数。

当使用了自己的render之后，在这个组件内Vue原生的`v-if`等都不再起作用。

过滤器可以用在mustache表达式中或者`v-bind`中，接受左边的值作为第一参数，处理后以返回值的方式返回。

## TypeScript支持

为了使用ts的类型检查，需要用`Vue.component`或`Vue.extend`定义组件。

## 数据驱动异步更新

由于Vue的数据是异步更新的，如果要在绑定的数据更新之后再进行一些操作，使用`Vue.nextTick(callback)`或`vm.$nextTick(callback)`。

