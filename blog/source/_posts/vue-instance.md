---
title: Vue的Instance和Template语法
date: 2018-10-03 13:23:44
tags: [vue]
categories: 软工
---

<https://vuejs.org/v2/guide/instance.html>

### MVVM

> 这意味着视图模型负责从模型中暴露（转换）[数据对象](https://zh.wikipedia.org/wiki/%E5%AF%B9%E8%B1%A1_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6))，以便轻松管理和呈现对象

<https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel>

我的感受：数据驱动不同于事件驱动的关键：View与Model之间的数据绑定。

### Vue Instance里能有的选项

- [ ] <https://vuejs.org/v2/api/#Options-Data>

#### data

data里的变量应该语义上都只是data，不推荐是一个有复杂方法的对象。

动态添加的data的attribute没有reactive的效果，最后在`new Vue({`的时候就全部声明。

创建后data对象用`vim.$data`访问，它的不以下划线`_`开头的属性都会被直接代理到`vm`上。

对于`Vue.component`的data，如果不想让所有同类component之间共享这个变量，要定义成立即返回一个新对象的函数。

有一种在js里deep copy的sao操作是`JSON.parse(JSON.stringify(...))`。

### 生命周期回调

一个Vue instance会有一个完整的生命周期如图

![vue lifecycle](https://vuejs.org/images/lifecycle.png)

在周期的每个部分有一些Hook事件，用`function(){`编写响应事件的回调时，`this`是当前的Vue Instance。

我发现HTML Element的`InnerHTML`不包括它本身，`OuterHTML`包括它本身。

另外如果Vue Instance带有`template`参数，那么它将完全**替换**掉原来的`el`元素，和之前的`el`元素的TagName和内容都被替换掉了，如<https://jsfiddle.net/gerwang/wv8ndtjk/5/>。

## 模板语法

`v-once`可以让一个element上的所有属性都只绑定成它的初值并不再改变（类似后端模板引擎的行为）。

`{ { rawHtml }}`会自动escape保证它按照字符串的方式被显示。观察<https://jsfiddle.net/gerwang/qz4k1pj7/5/>，发现如果使用`v-html`，那么该element的**InnerHTML**被替换成`rawHtml`，并且仍然是**绑定**的。

在template里面，再使用`v-html`等操作不再有reactive的效果。

对于attribute，只能用`v-bind`绑定而不能用`{ { }}`绑定。

在绑定id上，我遇到了一个问题，见<https://jsfiddle.net/gerwang/978jxv2n/17/>，似乎vue的virtual DOM的render是异步的，导致id改变后用原生的DOM操作只能找到旧的id，过一段时间才能找到新的id。

### Directives

冒号`:`表示一些指令需要的参数。

`.`表示修饰符，比如`v-on:submit.prevent`表示这个响应函数要自动调用`e.preventDefault()`。

`:href`是`v-bind:href`的缩写。

`@click`是`v-on:click`的缩写。











