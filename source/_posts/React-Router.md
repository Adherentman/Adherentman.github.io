---
title: React-Router v3
date: 2017-07-20 23：47
comments: true
layout: post
tags: [JavaScript,React]
categories: React
---

# React-Router3



### 路径语法

路由路径是匹配一个（或一部分）URL 的 [一个字符串模式](https://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#routepattern)。大部分的路由路径都可以直接按照字面量理解，除了以下几个特殊的符号：

- `:paramName` – 匹配一段位于 `/`、`?` 或 `#` 之后的 URL。 命中的部分将被作为一个[参数](https://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#params)
- `()` – 在它内部的内容被认为是可选的
- `*` – 匹配任意字符（非贪婪的）直到命中下一个字符或者整个 URL 的末尾，并创建一个 `splat` [参数](https://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#params)

<!--more-->

```react
//匹配 /hello/michael 和 /hello/ryan
<Route path="/hello/:name">         
  
  
//匹配 /hello, /hello/02 和 /hello/01
<Route path="/hello(/:id)">   


//匹配 /files/hello.jpg和/files/path/to/hello.jpg
<Route path="/files/*.*">       
```

## Histories

常用的 **history** 有三种形式， 但是你也可以使用 React Router 实现自定义的 history。

- [`browserHistory`](https://react-guide.github.io/react-router-cn/docs/guides/basics/Histories.html#browserHistory) (推荐)
- [`hashHistory`](https://react-guide.github.io/react-router-cn/docs/guides/basics/Histories.html#hashHistory)
- [`createMemoryHistory`](https://react-guide.github.io/react-router-cn/docs/guides/basics/Histories.html#creatememoryhistory)

```react
<Router history={browserHistory}>
```

### `browserHistory`

Browser history 是使用 React Router 的应用推荐的 history。它使用浏览器中的 [History](https://developer.mozilla.org/en-US/docs/Web/API/History) API 用于处理 URL，创建一个像`example.com/some/path`这样真实的 URL 。





## 在组件外部使用导航

虽然在组件内部可以使用 `this.context.router` 来实现导航，但许多应用想要在组件外部使用导航。使用Router组件上被赋予的history可以在组件外部实现导航。