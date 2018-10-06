---
title: Vue组件高级
tags: [vue]
date: 2018-10-03 21:00:22
categories: 软工
---

https://vuejs.org/v2/guide/components-registration.html

### 注册

使用ES2015的话，可以用`import A from './A.vue'`的方法来导入单文件组件并`export default`出去。`components:`属性存放能够在template中使用的component对象。

## Props

props除了可以是数组，也可以是object，每个key是变量名，value是变量类型（用类的prototype表示）。

如果将所有参数放在一个object里面，如`post`，那么`v-bind="post"`会将post的属性分别绑定。

由于parent的值在改变时会传递到children但不能反向传播，所有不要试图在children中修改props变量的值。

注意在做prop validation的时候，component还没有被创建，所以`data`、`computed`等里面的东西的不可用的。

对于没有被加到props列表里的属性，会自动的被加到root element上。

如果template中已经有class或者style了，再提供的class或style会被Vue较智能的合并。

如果设置component的`inheritAttrs: false`的话，可以用`$attrs`把不在props里的属性导向指定的element。

## Custom Events

总是用kebab-case命名event。

可以用`model`来指定` v-model`的行为，比如

```js
model: {
    prop: 'checked',
    event: 'change'
  }
```

会将value传递给props中的checked，当emit一个change event的时候被更新。

我们可以使用`$listeners`可以将component监听的事件**回调**导向一个指定的element。

用`.sync`可以帮助我们进行双向绑定，注意`$emit`的事件名有固定的格式。一个例子见<https://jsfiddle.net/gerwang/m7yuk2cn/24/>。

## Slot

slot可以具名。在component的template中写`<slot name="A">`，然后在parent环境中写`<div slot="A">`可以把这个元素导向同名的slot。所有无slot属性的元素都会被导向默认的不具名的`<slot>`上。

在`<keep-alive>`标签中，失活的组件会被缓存起来，在再次激活时不需要创建新的element。

## sao操作们

可以用`$root`访问根Vue实例，用`$parent`访问父级Vue实例。

用`ref=“A”`指定的名字可以在子环境中用`$refs.A`来访问这个对象。

### 依赖注入

在祖先组件的`provide:`里注入变量，在子孙组件的`inject:`里填入对应的变量名，那么就可以在任意深度的子孙里使用这些变量（这不是reactive的，所以不要修改）。

### 更新控制

使用`$forceUpdate`进行强制更新，在template中使用`v-once` attribute来让一个元素不会被更新。

