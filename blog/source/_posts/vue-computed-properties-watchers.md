---
title: Vue中的computed、绑定和v-if
tags: [vue]
date: 2018-10-03 15:47:03
categories: 软工
---

## Computed Properties

Computed properties存放在`computed:`里面，Vue会生成一个只读属性，调用时按属性的方式来调用（不加`()`）。只有当`computed`的依赖发生改变时，缓存才会刷新。

对于`Date.now()`这样不断自动刷新的信息，不应该用`computed`而应该用`methods`。

### Watchers

使用Watcher可以更精细的监控数据的变动，watcher的签名一般是`function(newValue, oldValue){`。

## Class and Style Bindings

### Object Syntax

对象既可以在`v-bind:class=`的字符串里进行构造，也可以是一个javascript上下文中存在的对象，每个key对应的value要保证是boolean。也可以绑定computed property。

### Array Syntax

可以用字符串列表把多个字符串拼接起来，拼接时也可以和Object Syntax进行拼接。

### Style Bindings

对于不同浏览器需要不同前缀的css属性，Vue会自动提供多种前缀的属性。

现在可以为一个属性提供一个列表，Vue会选择**浏览器支持的最后一个属性值**进行渲染，可以提高跨浏览器的兼容性。

## Conditional Rendering

可以用`<template>`作为`v-if`的大括号。`v-else`和`v-else-if`必须和`v-if`同级且紧跟其后。

### 用key控制可重用性

在`v-if`和`v-else`之中，如果使用的elements的类型是相同的，那么Vue会进行重用（条件发生改变时不会销毁现有的element再去创建新的element）。如果你不想Vue重用element的话，可以给element不同的`key`属性，这样它们就被认为是不同的。

```html
<template v-if="loginType === 'username'">
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <input placeholder="Enter your email address" key="email-input">
</template>
```

如以上两个`<input>`被认为是不同的。

### `v-show`

相比`v-if`的销毁与重建，`v-show`只是改变`display`属性来改变可见性，`v-show`的切换性能要更好一些。