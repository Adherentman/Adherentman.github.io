---
title: Ajax与Fetch？
date: 2017-03-20 22：47
comments: true
layout: post
tags: [JavaScript]
categories: Javascript修仙之旅
---

虽说现有Fetch和Axios，但我们还是需要了解下当初那传奇的Ajax！

## Ajax

Ajax是基于XMLHttpRequest对象来请求数据的。所以我们需要先了解下XMLHttpRequest对象。
<!--more-->
当我们发起一个请求时：

```javascript
function reqListener () {
  console.log(this.responseText);
}
let xhr = new XMLHttpRequest();
xhr.onload = reqListener;
xhr.open("get", "yourfile.txt", false)
xhr.send();
```

那么逐条解释，

XMLHttpRequest简称XHR。

在XHR的属性有`responseText`，也就是代码第2行中，它是作为*响应主体被返回的文本*，而且无论内容类型是什么，它们都会保存在该属性中。

<hr/>

第4行代码，构造函数XMLHttpRequest初始化了一个XMLHttpRequest对象。

<hr/>

第5行代码，看以下图：

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/XMLHttpRequest.png)

我们会发现`onload`是XMLHttpRequestEventTarget的事件处理程序的接口。

* [XMLHttpRequestEventTarget.onabort](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onabort)
  * 当请求失败时调用该方法
* [XMLHttpRequestEventTarget.onerror](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onerror)
  * 当请求发生错误时调用该方法
* [XMLHttpRequestEventTarget.onload](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onload)
  * 当一个HTTP请求正确加载出内容后返回时调用。
* [XMLHttpRequestEventTarget.onloadstart](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onloadstart)
  * 当一个HTTP请求开始加载数据时调用。
* [XMLHttpRequestEventTarget.onprogress](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onprogress)
  * 间歇调用该方法用来获取请求过程中的信息。
* [XMLHttpRequestEventTarget.ontimeout](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/ontimeout)
  * 当时间超时时调用；只有通过设置XMLHttpRequest对象的timeout属性来发生超时时，这种情况才会发生。
* [XMLHttpRequestEventTarget.onloadend](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onloadend)
  * 当内容加载完成，不管失败与否，都会调用该方法

> 参考资料[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget)

<hr/>

第6行就都是XMLHttpRequest对象的**方法**。

* open(method, url, async, user, password)
  * 请求使用Http的方法，如method = 'GET' | 'POST' | 'PUT' | 'DELET'
  * url就是url
  * async： boolean。是否异步操作。如果为false只有你得到服务器返回的数据send()方法才会返回数据。如果为值为true，一个对开发者透明的通知会发送到相关的事件监听者。
  * user，password: 可选参数,为授权使用
* 另外在我们**接收到响应之前**还可以调用abort()方法来取消异步请求。

<hr/>

当然XMLHttpRequest对象也有一些重要的属性而且这个对象也继承了EvenTarget和XMLHttpRequestEvenTarget的属性（如上图所示）。

最重要的是当**readyState**属性的值由一个值变为另一个值得时候，都会触发一次**readystatechange**事件。

那么**readyState**有哪些值呢？

* 0，未初始化。还没调用open()方法
* 1，启动。已经调用了open()方法
* 2，发送。已经调用了send()方法
* 3，接受。已经接收到部分响应数据
* 4，完成。已经接收到全部的响应数据，而且已经可以再客户端使用了。

我们一般对值为4这个阶段感兴趣。因为这时候所有的数据已经到位了，我们就可以做许多事情了。比如说：

```Javascript
let xhr = new XMLHttpRequest();
xhr.onreadystatechange = function(){
    if(xhr.readyState == 4){
        balbalbal......
    }
}
```

还有**response**，**responseText**，**responseType**，**responseXML**，**status**，**statusText**，**statusText**，**withCredentials**属性。

## Fetch

### Fetch是什么？

Fetch 是浏览器提供的原生 AJAX 接口。使用 window.fetch 函数可以代替以前的 $.ajax、$.get 和 $.post。

Fetch还提供了单个逻辑位置来定义其他HTTP相关概念，例如 **CORS**和HTTP的扩展。

请注意，fetch 规范与 jQuery.ajax() 主要有两种方式的不同，牢记：

* 当接收到一个代表错误的 HTTP 状态码时，从 `fetch()`返回的 Promise **不会被标记为 reject，** 即使该 HTTP 响应的状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve （但是会将 resolve 的返回值的 ok 属性设置为 false ），  仅当网络故障时或请求被阻止时，才会标记为 reject。
* 默认情况下, `fetch` **不会从服务端发送或接收任何 cookies**, 如果站点依赖于用户 session，则会导致未经认证的请求（要发送 cookies，必须设置 [credentials](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalFetch/fetch#%E5%8F%82%E6%95%B0) 选项）.

> 参考资料[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)

### 例子

```javascript
fetch('/').then(function(response){
    response.text().then(function(text){
        console.log(text)
    })
})
```

这是个很简单的Fetch的小例子。

.then…这不是和Promise好像吗？

没错。Fetch API就是基于Promise的。但是因为基于Promise所以我们无法像XHR一样调用`.abort()`去中断请求。。

### 自定义请求的参数

那么，Fetch第二个参数我们可以传一个可以控制不同配置的 `init` 对象：

```javascript
var myInit = { method: 'GET'|'POST'|'PUT'|'DELETE',
               headers: myHeaders,
               mode: 'cors',
               cache: 'default' };
```

* `mode`: 请求的模式，如 `cors、` `no-cors 或者` `same-origin。`
* `cache`:  请求的 cache 模式: `default `、 `no-store 、` `reload 、` `no-cache 、` `force-cache `或者 `only-if-cached 。`

其他可以去参考MDN。

### header

在Fetch下可以很方便的操作header：

```javascript
fetch(url).then(function(responen){
    console.log(responen.headers.get('Content-Type'));
})
```

```javascript
var myHeaders = new Header();
myHeaders.append("Content-Type", "text/html");
fetch(url).then(function(){
    headers: myHeaders
})
```

```javascript
var header = new Headers({
  "Content-Type": "text/plain"
});
console.log(header.has("Content-Type")); //true
console.log(header.has("Content-Length")); //false
```

