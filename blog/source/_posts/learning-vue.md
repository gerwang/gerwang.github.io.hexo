---
title: Vue安装和简介
date: 2018-10-02 22:39:41
tags: [vue]
categories: 软工
---

看一看<https://vuejs.org/>主页的那个视频可以快速了解vue的特色。我注意到vue有一个帮助我们调试的chrome插件，在这里[vue-devtools](https://github.com/vuejs/vue-devtools#vue-devtools)。

<https://vuejs.org/v2/guide/installation.html>

[UMD](https://github.com/umdjs/umd)是一种结合AMD与CommonJS语法的东西，如果直接在浏览器里面用`<script>`调用就用它。用新的webpack的话用ES Module格式的。

如果在`new Vue`里面没有传`template`，在runtime时不需要compiler，这样更轻量级，更好。

通过`process.env.NODE_ENV`确定打包时是开发还是生产模式。

<https://vuejs.org/v2/guide/index.html>

Vue的特点是可以先部分使用Vue，再逐渐增加使用的范围。

这里有各大数据驱动框架的对比<https://vuejs.org/v2/guide/comparison.html>，等框架都用过之后再来看。

### Vue常用directive

`el`是绑定Component和element的CSS选择器。

在属性前加`v-bind:`可以让data绑定attribute，attribute里填写的是data的变量名。

`v-if`

在使用`v-for`时要注意，是写在需要**复制**的那个element里面，比如

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">{{ todo.text }}</li>
  </ol>
</div>
```

是写在`<li>`上而不是`<ol>`上。

能被html5调用的函数存放在`methods`里面。

为了能够正确的绑定`this`，method的值应该用`function() {`定义。在这个`this`对象里，`data`和`method`里的东西都直接成为了它的属性，如<https://jsfiddle.net/gerwang/vpdqt4L2/10/>。

要使用`v-model`才是双向的绑定，如果关联`<input>`的话，不需要点提交按钮，修改是实时的。

Vue的component像是在H5上还原了桌面GUI程序的设计方法。

`props`可以把parent作用域的变量名传到component的作用域中。注意即使component中props的变量名正好和parent域中的data的变量名相同，仍然要使用`v-bind`来绑定一下。如<https://jsfiddle.net/gerwang/efxt0a83/17/>。<del>另外我发现，凡是用`v-bind`操作绑定的属性，都不会出现在DOM树的属性中</del>UPD：似乎如果是HTML支持的属性，比如`id`，`v-bind`后仍然会出现在DOM中（可以在开发者工具Elements里面找到）。https://jsfiddle.net/gerwang/qz4k1pj7/5/