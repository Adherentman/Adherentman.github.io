---
title: React-Saga笔记
date: 2018-03-31 22:55
comments: true
layout: post
tags: [React]
categories: React
---
## 监听

### takeEvery:

```Javascript
function* watchFetchData() {
  yield* takeEvery('FETCH_REQUESTED', fetchData)
}
```

允许多个`fetchData`同时进行，即使上一个还没执行完成。
<!--more-->

### takeLatest：

```javascript
function* watchFetchData() {
  yield* takeLatest('FETCH_REQUESTED', fetchData)
}
```

始终执行最新的那个请求的响应。



总结一下就是：

我们疯狂点击一个按钮，它触发也疯狂触发了多次`fetchData`。

如果我们是`takeEvery`那将你点击几次他执行几次`fetchData`。

如果我们是`takeLatest`那将只执行你最后点击的那一次`fetchData`。



##Effect

从 Generator 里 yield 纯 JavaScript 对象以表达 Saga 逻辑。 我们称呼那些对象为 *Effect*。

可以使用 `redux-saga/effects` 包里**提供的函数**来创建 Effect。

### call：

`call(fn, ...args)` 这个函数。现在我们不立即执行异步调用，相反，call 创建了一条描述结果的信息

简单来说，call就是redux里的Action Creator。 



###put：

这个函数用于创建 dispatch Effect。

```javascript
//before
function* fetchProducts(dispatch)
  const products = yield call(Api.fetch, '/products')
  dispatch({ type: 'PRODUCTS_RECEIVED', products })
}

//after
import { call, put } from 'redux-saga/effects';

function* fetchProducts() {
  const products = yield call(Api.fetch, '/products')
  // 创建并 yield 一个 dispatch Effect
  yield put({ type: 'PRODUCTS_RECEIVED', products })
}
```

### take：

它将会暂停 Generator 直到一个匹配的 action 被发起。

```javascript
function* loginFlow() {
  while(true) {
    yield take('LOGIN')
    // ... perform the login logic
    yield take('LOGOUT')
    // ... perform the logout logic
  }
}
```

也就是说我们可以用`take`去监听还未发生的事情。比如我们Login之后必定跟着一个Logout，或者说我们Logout后面必定跟着一个Login。

