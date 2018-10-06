---
title: 学习Vuex
tags: [vuex]
date: 2018-10-06 10:28:33
categories: 软工
---

在我的理解中，Vuex可以将单页应用中的数据统一存放，这样的话所有view都从同一个地方取数据。

可以在定义state的时候使用`mapState`为一些常用的state设置别名，在子组件中使用`...mapState(['count'])`将state混入其他属性当中。

如果在`getter`里面返回了一个函数，每次都会取出这个函数进行计算，计算的值不会被缓存。

在commit的时候可以把参数作为一个object传过去，也可以将mutation的名字以`{type:'increment'}`的形式传递过去。

为了能在devtools中正常的查看，在mutation中不要使用异步函数。

在Vuex里父子关系也可以被表示成类似文件系统的路径形式。可以用`createNamespacedHelpers`快速得到某个子树的`mapState`等函数。

可以使用`registerModule`方法在store产生之后追加module。

一个module如果要被多次重用，其中的`state`域要是一个函数来避免module之间共享变量。

开启`strict: true`以后Vuex对于不是mutation引起的状态改变会报错，但是在production下应该关掉以提高性能。

在使用`v-model`时，为了使用Vuex应该传入一个有专门get和set方法的property。

