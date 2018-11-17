---
title: Contenteditable与变成输入普通文本
date: 2018-04-15 19:36:26
updeated: 2018-04-17 23:42:01
comments: true
layout: post
tags: [Html]
categories: Html
---

我们需要实现富文本的方法有一个很方便那就是在任意元素你都可以给个属性叫`contenteditable`

```html
<div contenteditable>我是个富文本哟！</div>
```

很简单这就是一个富文本啦！

<!--more-->

经过百度一番，看见张鑫旭大神的很久以前的文章。为了实现普通输入文本我们只能借助css啦！

**user-modify**

* user-modify: read-only;
  * 只能看
* user-modify: read-write;
  * 支持富文本
* ~~user-modify: write-only;~~
  * 无所谓的东西
* user-modify: read-write-plaintext-only;
  * 纯文本

OK~~问题解决啦。那就是只要给那个元素设个css

```html
<div contenteditable style="user-modify:read-write-plaintext-only">
    我是纯文本
</div>
```



参考：

* [张鑫旭大神博客](http://www.zhangxinxu.com/wordpress/2016/01/contenteditable-plaintext-only/)