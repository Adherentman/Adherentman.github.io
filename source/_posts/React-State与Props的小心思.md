---
title: React State与Props的小心思
date: 2017-06-30 12:10
comments: true
layout: post
tags: [JavaScript,React,Webpack]
categories: React
---

忙于期末考。。很久都没有更新博客了！因为找了个实习需要React所以这里写写自己在看React的官方文档中，遇到的问题。

# Props与State很容易让我经常弄混

## 先来说说Props

- 官方解释

组件从概念上看就是函数，之后这个组件可以接受任意的输入值，并返回一个需要页面上展示的React元素。那么这个输入值就为**props。**

- 个人理解

**props**是不可变的，传入什么值进去，最后返回的也是传入的值。也就是说，只读。

<!--more-->

### props父子传递

```javascript
function Uesr(props){
  return(
  <div className={'abc'+ props.color}>
    {props.children}
  </div>
  );
}
------------------------------------------------------------------------
  
function My(){
  return(
  <User color="red">
    <h1>nihao</h1>
  </User>
  );
}
```

在JSX标签内的任何内容都将通过`children`属性传入`User`。因为`User`在一个`div`内渲染了`{props.children}`，所以被传递的所有元素都会出现在最终输出中。

其实，我们也可以不用children。借用React官方文档的例子：

```javascript
function Contacts() {
  return <div className="Contacts" />;
}

function Chat() {
  return <div className="Chat" />;
}

function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}

```

## 接下来State

### state怎么工作？

通过调用setState(data,callback)方法，改变状态，就会触发React更新UI。

### 什么时候组件需要state呢？

一般来说，大部分的组件应该从`props`属性中获取数据然后渲染。那么在！

**用户输入，服务器交互，这些情况下会用到State**。在官方上说，**尽可能的保持你的组件无状态化。**

通过看官方文档。。我发现他们的模式是：构建几个无状态的组件用来渲染数据，然后在这些之上去构建一个有状态的组件同用户和服务器交互，数据通过props传递给无状态组件。

- setState:更新组件状态。
- setState会触发diff算法：判断state和页面结果的区别，是否需要更新。



### 状态(state)和属性(props)对比

- 状态和属性都会触发render更新，都是纯JS对象
- 状态：是和自己相关的，既不受父组件也不受子组件影响
- 属性：本身是不能自己去修改的，只能从父组件获取属性，父组件也能修改它的属性
- 根本的区别：组件在运行时需要去修改维护的就是状态