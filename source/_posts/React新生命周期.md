---
title: React新生命周期
date: 2018-04-10 20:39:26
updated: 2018-04-11 21:41:42
comments: true
layout: post
tags: [React]
categories: React
---

# React16.3.1

React发生了重大的变化。并且更新了新的生命周期我们来了解一下。

献上一个图！

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/react%E6%96%B0%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

> 地址：[React新生命周期图](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

<!--more-->

# 生命周期

## Mounting/挂载

* constructor()
* static getDerivedStateFromProps()
* UNSAFE_componentWillMount()
* render()
* componentDidMount()



## Updating/更新

* UNSAFE_componentWillReceiveProps()
* static getDerivedStateFromProps()
* shouldComponentUpdate(prevProps, prevState)
* UNSAFE_componentWillUpdate()
* render()
* getSnapshotBeforeUpdate()
* componentDidUpdate(prevProps, prevState, snapshot)


## Unmounting/卸载

* componentWillUnmount()



# Demo-getDerivedStateFromProps&&componentDidUpdate:

链接： [小demo，了解一下](https://codesandbox.io/s/2xv69l269j)

componentDidUpdate是个做网络请求的好地方。

```javascript
import React from "react";
import { render } from "react-dom";

const App = () => (
  <div style={styles}>
    <Hello name="CodeSandbox" />
    <h2>Start editing to see some magic happen {"\u2728"}</h2>
    <B />
  </div>
);
class B extends React.Component {
  state = {
    myParentValue: ""
  };

  componentDidMount() {
    setTimeout(() => {
      this.setState({ myParentValue: "XuZiHao" });
    }, 1000);
  }

  render() {
    return <C foo={this.state.myParentValue} />;
  }
}

class C extends React.Component {
  state = {
    foo: ""
  };

  static getDerivedStateFromProps(nextProps, prevState) {
    if (nextProps.foo === "XuZiHao") {
      return {
        foo: "bar"
      };
    }

    return null;
  }

  componentDidUpdate() {
    if (this.state.foo === "bar") {
      setTimeout(() => {
        this.setState({ foo: "Hello!!!!" });
      }, 4000);
    }
  }

  render() {
    return <h1>Value of `foo` is {this.state.foo}</h1>;
  }
}
render(<App />, document.getElementById("root"));

```



# Demo-shouldComponentUpdate(prevProps, prevState)

```javascript
import React from 'react'
class Test extends React.Component{
  constructor(props) {
    super(props);
    this.state = {
      Number:1
    }
  }
  //这里调用了setState但是并没有改变setState中的值
  handleClick = () => {
     const preNumber = this.state.Number
     this.setState({
        Number:this.state.Number
     })
  }
  //在render函数调用前判断：如果前后state中Number不变，通过return false阻止render调用
  shouldComponentUpdate(nextProps,nextState){
      if(nextState.Number == this.state.Number){
        return false
      }
  }
  render(){
    //当render函数被调用时，打印当前的Number
    console.log(this.state.Number)
    return(<h1 onClick = {this.handleClick} style ={{margin:30}}>
             {this.state.Number}
           </h1>)
  }
}
```



# Demo-getSnapshotBeforeUpdate()

官方例子：

> [官网地址](https://reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)

```javascript
class ScrollingList extends React.Component {
  listRef = React.createRef();

  getSnapshotBeforeUpdate(prevProps, prevState) {
    // 是否将新项目添加到列表中？
    // 捕获列表的当前高度，以便稍后调整滚动。
    if (prevProps.list.length < this.props.list.length) {
      return this.listRef.current.scrollHeight;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    // 如果我们有快照值，我们刚刚添加了新items
    // 调整滚动条，以便这些新items不会将旧的推出视图。
    if (snapshot !== null) {
      this.listRef.current.scrollTop +=
        this.listRef.current.scrollHeight - snapshot;
    }
  }

  render() {
    return (
      <div ref={this.listRef}>{/* ...contents... */}</div>
    );
  }
}
```

