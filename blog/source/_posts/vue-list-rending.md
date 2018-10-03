---
title: Vue中的v-for、事件处理、表单绑定、组件基础
tags: [vue]
date: 2018-10-03 16:57:37
categories: 软工
---

https://vuejs.org/v2/guide/list.html

### v-for

`v-for="(item, index) in items"`可以获得当前item的下标。

`v-for="(value, key, index) in object"`可以迭代js对象。

`v-for`的默认实现中如果出现列表长度改变需要插入删除时，会就地复用原来的elements，这对于一些依赖于DOM element隐含属性的情况会出现问题。为了避免这种行为，给每个element赋上不同的`key` attribute。

### 一些限制

有些数组的变化不能被Vue侦测到，需要用一些替代的写法：

- `items[idx]=val`改为`Vue.set(items,idx,val)`或`items.splice(idx,1,val)`。
- `items.length=newLen`改为`items.splice(newLen)`。

其中`array.splice(start[, deleteCount[, item1[, item2[, ...]]]])`表示从`start`开始先向后删除`deleteCount`个元素，再向后插入`item1, item2, ...`。

对于对象的限制：

- 添加新的属性用`Vue.set(obj,key,value)`。
- 如果要合并两个对象的属性，使用`vm.a=Object.assign({},vm.a,b)`来用一个新对象来替换旧的`vm.a`。

如`v-for="idx in 10"` 是一个integer，那么`idx`将从1循环到10。

如果用`v-for`列举component，必须要提供`key` attribute。

在使用component时，用`is="todo-item"`相当于把TagName设成了`todo-item`。

## 事件处理

如果在html的`@click`中就需要使用事件处理函数的`event`参数，可以用`$event`访问。

#### 事件修饰符

- `.stop`调用`event.stopPropagation()`停止传播
- `.prevent`调用`event.preventDefault()`避免默认的行为
- `.capture`事件先被自己处理，再交给子级handler处理
- `.self`只有`event.target`是自己（`this`）时才触发
- `.once`最多触发一次
- `.passive`事件的默认行为不等待处理函数完成，而是立即触发（用来提高性能）。

Vue里有监听键盘按键的modifier，但是我还不大会用。

## 表单处理

使用`v-model`时，始终在javascript里面定义初值，HTML里的初值是被忽略的。

在使用多选框checkbox时，`v-model`填数组名，`value`填被勾中时会往数组里加入的值。

`.number`修饰符可以让表单的结果被parse成float，如果不能parse的话就返回原始的值。

## 组件基础

如果在一个HTML内用到了custom component，它的父亲一定要被包在一个`new Vue`里面（如`new Vue({ el: '#components-demo' })`）。

`$emit(eventName,arg)`可以给自己的父component发事件，其中`arg`会作为inline环境的`$event`和处理函数里的`event`参数。

#### 如何让自己的component支持`v-model`

首先`v-model`作用在custom component时，本质上做了两件事：

```
<custom-input  v-bind:value="searchText"  v-on:input="searchText = $event"></custom-input>
```

只要显示好上下文中的`value`变量并且在输入时触发`input`事件就可以了。

一个例子在<https://jsfiddle.net/gerwang/db9c7kte/24/>，注意几点：

- Vue的component只能有一个root element。
- `value`属性虽然没有显式绑定（隐式的通过`v-model`绑定），但是`props`里面要填`value`。

`<slot>`可以把传进custom component的innerHTML传到某个指定的地方。

可以绑定`<component>`的`is`属性来动态切换组件的类型。不过我在<https://jsfiddle.net/gerwang/o3nycadu/18123/>里添加了三个`<input>`，发现在每次component切换时，旧的element都被销毁了，`<input>`里暂存的输入数据也消失了。不过如果`<input>`被绑定上了变量，切换回来时应该就能重新加载上。

