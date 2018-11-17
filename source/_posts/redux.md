---
title: React-Redux和Redux
date: 2017-07-31 09:18:26
comments: true
layout: post
tags: [JavaScript,React]
categories: React
---

react无法让两个组件互相交流，使用对方数据。

# Redux

- 需要回调通知state（等同于回调参数）->action
- 需要根据回调处理（等同于父级方法） ->reducer
- 需要state(等同于总状态) ->store

现在您只需要记住 `reducer` 是一个函数，负责更新并返回一个新的`state`

而 `initialState` 主要用于前后端同构的数据同步

<!--more-->

## Action

- 是把数据从应用传到store的有效载荷。
- 是store数据的唯一来源
- 描述发生了什么的普通对象
- 也可以理解成新闻的摘要-“任务列表里添加了学习Redux文档”。

## Store

- Redux应用中只有一个单一的store
- 维持应用的state
- 提供 getState() 获取state
- 提供dispatch() 更新state
- 通过subscribe(listener) 注册监听器
- 通过subscribe(listener) 返回的函数注销监听器.
- 会把2个参数传入reducer：当前的state树和action。

## Reducer

## Reducer

reducer就是实现(state,action) -> newState的纯函数. 也就是真正处理state的地方.

Redux不希望我们修改老的state ,而且通过直接返回新的state的方式去修改.

- **永远不要**在 reducer 里做这些操作：
  - 修改传入参数；
  - 执行有副作用的操作，如 API 请求和路由跳转；
  - 调用非纯函数，如 `Date.now()` 或 `Math.random()`。
- 指明根据action更新state。



通俗点讲，就是 `reducer` 返回啥，`state` 就被替换成啥

- view(React)
- store(state)
- action
- reducer


- view(React) = 家具的摆放在视觉的效果上
- store(state) = 每个家具在空间内的坐标(如：电视的位置是x:10, y: 400)
- action = 小明分配任务(谁应该干什么)
- reducer = 具体任务都干些什么(把电视搬到沙发正对面然后靠墙的地方)

所以这个过程应该是这样的：

**view ---> action ---> reducer ---> store(state) ---> view**



# React-Redux

1. Provider是一个普通组件，可以作为顶层app的分发点，它只需要store属性就可以。他会将state分发给所有被connect的组件，不管它在哪里，被嵌套多少层。

2. connect是真正的重点，它是一个科里化函数，意思是先接受两个参数（数据绑定mapStateToProps和事件绑定mapDispatchToProps），再接受一个参数（将要绑定的组件本身）：mapStateToProps：构建好Redux系统的时候，它会被自动初始化，但是你的React组件并不知道它的存在，因此你需要分拣出你需要的Redux状态，所以你需要绑定一个函数，它的参数是state，简单返回你关心的几个值。

3. mapDispatchToProps：声明好的action作为回调，也可以被注入到组件里，就是通过这个函数，它的参数是dispatch，通过redux的辅助方法bindActionCreator绑定所有action以及参数的dispatch，就可以作为属性在组件里面作为函数简单使用了，不需要手动dispatch。这个mapDispatchToProps是可选的，如果不传这个参数redux会简单把dispatch作为属性注入给组件，可以手动当做store.dispatch使用。这也是为什么要科里化的原因。

   做好以上流程Redux和React就可以工作了。简单地说就是：

   ​

   1.顶层分发状态，让React组件被动地渲染。

   ​

   2.监听事件，事件有权利回到所有状态顶层影响状态。

# Redux 与传统后端 MVC 的对照

| Redux                         | 传统后端 MVC                           |
| ----------------------------- | ---------------------------------- |
| `store`                       | 数据库实例                              |
| `state`                       | 数据库中存储的数据                          |
| `dispatch(action)`            | 用户发起请求                             |
| `action: { type, payload }`   | `type` 表示请求的 URL，`payload` 表示请求的数据 |
| `reducer`                     | 路由 + 控制器（handler）                  |
| `reducer` 中的 `switch-case` 分支 | 路由，根据 `action.type` 路由到对应的控制器      |
| `reducer` 内部对 `state` 的处理     | 控制器对数据库进行增删改操作                     |
| `reducer` 返回 `nextState`      | 将修改后的记录写回数据库                       |



# 总结

## redux 三个基本原则

1. 整个应用只有唯一一个 Store 实例
2. State 只能通过触发 Action 来更改
3. State 的更改 必须写成纯函数(Reducer)，(oldState, action) => newState，也就是每次更改总是返回一个新的 State

## redux 两个显著的特点

1. 可预测性（Reducer 是纯函数）。
2. 扩展性强（middleware）。


## reducer 可以根据场景分为以下几种:

- root reducer :根reducer ,作为createStore的第一个参数
- slice reducer : 分片reducer,相对根reducer 来说的.用来操作state的一部分数据.多个分片reducer可以合并成一个根reducer.
- higher-order reducer : 高阶reducer 接受reducer作为函数/返回reducer作为返回的函数.
- case function: 功能函数,接受指定action后的更新逻辑,可以是简单的reducer函数,也可以接受其他参数.

## reducer 的最佳实践主要分为以下几个部分

- 抽离工具函数,以便复用.
- 抽离功能函数(case function),精简reducer声明部分的代码
- 根据数据类别拆分,维护多个独立的slice reducer.
- 合并slice reducer.
- 通过crossReducer在多个slice reducer中共享数据.
- 减少reducer的模板代码.