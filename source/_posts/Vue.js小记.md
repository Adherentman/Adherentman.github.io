---
title: Vue.js小记
date: 2017-04-28 15:54
comments: true
layout: post
tags: [JavaScript,Vue.js]
categories: Vue
---

#Vue小记

## **v-bind**

- 缩写： `：`
- 修饰符
  - `.prop` - 被用于绑定DOM属性
  - `.camel`
- 用法

动态的绑定一个或多个特性，或一个组件`prop`到表达式

```javascript
<a v-bind:href="url" href="#"></a>
```

在这里`:href`是参数，通过`v-bind`指令将该元素的`href`属性与表达式的`url`绑定。

<!--more-->

## **v-if**

```javascript
<p v-if="seen">Now you see me</p>
```

这个`v-if`指令将判断`seen`的真假值来移出/插入`<p>`元素。



## **v-on**

- 缩写：`@`

```javascript
<a v-on:click="doSomething">
```

这个指令是用来监听DOM事件。

### 事件修饰符

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`



```html
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>

2.1.4新增
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```

### 按键修饰符

```html
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
记住keyCode各种值很难所以提供了别名
<!-- 同上 -->
<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

- `.enter`
- `.tab`
- `.delete`(捕获“删除”和“退格”按键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

**2.1.0新增**

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`(Mac上为(⌘)在windows上为(⊞))

## v-for

我们用 `v-for` 指令根据一组数组的选项列表进行渲染。 `v-for` 指令需要以

 `item in items` 形式的特殊语法， `items` 是源数据数组并且 `item` 是数组元素迭代的别名。

```html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```

```javascript
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      {message: 'Foo' },
      {message: 'Bar' }
    ]
  }
})
```

结果：

- Foo
- Bar

## v-model

你可以用 `v-model` 指令在表单控件元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖，它负责监听用户的输入事件以更新数据，并特别处理一些极端的例子。

具体看

[官方文档]: https://cn.vuejs.org/v2/guide/forms.html#vmodel-ime-tip	"V-model"

