---
title: 浅谈script标签
date: 2018-03-19 20：56
updated: 2018-04-01 20:21
comments: true
layout: post
tags: [JavaScript]
categories: Javascript修仙之旅
---

`<script>` ? 这不就是直接执行script脚本吗？

以前的我这有认为，但是今天却知道了他的奥秘。他并没有表面的那么简单。

<!--more-->

## 属性

`<script>`拥有7个属性，没想到吧！

* **async**
  * boolean
  * 异步执行该脚本，但不保证按照指定它们的先后顺序执行
* **defer**
  * boolean
  * 通知浏览器该脚本将在文档完成解析后遇到`</html>`，并会按照它们出现的先后顺序执行。但会在触发 `DOMContentLoaded` 事件前执行。
* **integrity**
  * 包含用户代理可用于验证已提取资源是否已无意外操作的内联元数据
* **src**
* **type**
* **text**
* **crossorigin**
  * 使那些将静态资源放在另外一个域名的站点打印错误信息,就是将跨域报错变为同源报错

> 在XHTML文档中，要把async属性设置为 async = "async", defer = "defer"

知道了那几个属性接下来，来看看下面这个图：

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/%3Cscript%3E.png)

1. 第一个光秃秃的`<script>`说明了，在`html`解析中，如果有`<script>`的话，`html`会在`Script`下载并且执行的时候，暂停解析。
2. 第二个带`async`属性的`<script>`，如图所示，也就是他下载`Script`的时候是异步的，但是只要`Script`文件下好了，那么就马上执行。
3. 第三个带`defer`属性的`<script>`，其实和上面带`async`属性一样都是异步执行去下载`Script`文件的。但是在这个带有`defer`的则是在`html`全部解析完毕之后才去执行`Script`文件。而且它是按照加载顺序执行脚本的，这一点要善加利用。显然 `defer` 是最接近我们对于应用脚本加载和执行的要求的

## 使用动态创建的`<script>`标签元素来下载并执行代码

```javascript
var script = document.createElement('script');
script.type = "text/javascript";
script.src = "file1.js";
document.getElementByTagName("head")[0].appendChild(script);
```

这个技术的重点在于：

无论何时启动下载，文件的下载和执行过程不会阻塞页面其他进程。

参考：

> 《高性能JavaScript》

## 使用XHR对象下载JS代码注入页面

```javascript
function loadScript(url, callback){
    var script = document.createElement(
"e");
    script.type = "text/javascript";
    if(script.readyState){	//ie
        script.onreadystatechange = function(){
            if(script.readyState == "loaded" || script.readyState == "complete"){
                script.onreadystatechage = null;
                callback();
            }
        };
    } else {
        script.onload = function(){
            callback();
        }
    }
}

loadScript("the-rest.js", function(){
    Application.init()
});
```

这样做实现了动态创建标签元素并下载，其次当第二个脚本文件下载时，应用所需的所有DOM结构已经创建完毕，并做好了交互的准备,从而避免了需要另一个事件来检测页面是否准备好。



## 参考

* [MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script)
* [SegmentFault](https://segmentfault.com/a/1190000006778717)

