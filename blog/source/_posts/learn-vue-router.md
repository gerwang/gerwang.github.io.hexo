---
title: 学习Vue Router
tags: [vue-router]
date: 2018-10-06 17:02:53
categories: 软工
---

用`<router-link to="/foo">`来生成路由的链接，用`<router-view>`指定被路由的组件的显示位置。

如果要在一个Vue实例里使用router记得注入`router`。

将冒号开头的单词放在`path`里面可以capture动态的path，就像django的url capture一样。

在`routes`中设置`children`可以根据URL决定多级的component。

> **注意：**如果目的地和当前路由相同，只有参数发生了改变 (比如从一个用户资料到另一个 `/users/1` -> `/users/2`)，你需要使用 [`beforeRouteUpdate`](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#%E5%93%8D%E5%BA%94%E8%B7%AF%E7%94%B1%E5%8F%82%E6%95%B0%E7%9A%84%E5%8F%98%E5%8C%96) 来响应这个变化 (比如抓取用户信息)。

可以给一条路由命名从而在`<router-link>`或者`router.push`里面很方便的引用。

`<router-view name="a">`可以实现在同一层级下有多个不同种类的component，其中没有`name`的那个`router-view`用`default`表示。

如果使用`new VueRouter({mode:'history'})`可以让router默认基于HTML5 History工作。

在<https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%AE%8C%E6%95%B4%E7%9A%84%E5%AF%BC%E8%88%AA%E8%A7%A3%E6%9E%90%E6%B5%81%E7%A8%8B>中有完整的路由导航的生命周期。

