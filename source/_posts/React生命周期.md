---
title: React生命周期
date: 2017-09-26 17:20
comments: true
layout: post
tags: [JavaScript,React]
categories: React
---

# React生命周期

生命周期可能经历如下三个经历：

> 装载过程(Mount): 把组件第一次在DOM树中渲染的过程；
>
> 更新过程(Update): 组件被重新渲染的过程;
>
> 卸载过程(Unmount): 组件从DOM中删除的过程;

<!--more-->

接下来，一个一个解释：

## 装载过程

1. constructor
2. getInitialState
3. getDefaultProps
4. componentWillMount
5. render
6. componentDidMount




### 1##

- 一个React组件需要constructor，往往是为了达到下面的目的
  - 初始化state，因为组件生命周期中任何函数都可能要访问state
  - 绑定成员函数的this环境
    - 举个例子：
    - this.onClickIncrementButton = this.onClickIncrementButton.bind(this);
    - 这条语句的作用，就是通过**bind方法**让当前实例中onClickIncrementButton函数被调用时，this始终是指向当前组件实例。

### 2##

- `getInitialState`和`getDefaultProps`是在React.createClass方法创造的组件类才会用到。

  - ```javascript
    const Sample = React.createClass ({
        getInitialState: function(){
            return {foo: 'bar'};
        },
      	getDefaultProps: function(){
            return {sampleProp: 0};
        }
    });	
    ```

- 在ES6的话

  - ```javascript
    class Sample extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
         foo: 'bar'
        };
      }
    Sample.defaultProps = {
      sampleProp: 0
    };
    ```

### 3##

- `render`函数是一个纯函数。完全根据this.state 和  this.props来决定返回的结果，而且不要产生任何副作用。
- 在render函数中去**调用this.setState毫无疑问是错误**的。因为一个纯函数不应该**引起状态的改变**。

### 4##

- 在装载过程中，`componentWillMount`会在调用`render`函数之前调用。
  - `componentDidMount`会在调用`render`函数之后调用。
- 这两个函数是`render`函数的前哨和后卫。
- 但是一般我们都不用定义`componentWillMount`,因为这个函数发生在"将要装载"的时候，这个时候没有任何渲染出来的结果。即使我们去调用this.setState修改状态也不会引发重新绘制。
  - 也就是说所有可以在`componentWillMount`中做的事情，我们都可以提前到`constructor`中去做。
- 我们来说说`componentDidMount`这个函数，它仅在浏览器端执行。


## 更新过程

1. componentWillReceiveProps
2. shouldComponentUpdate
3. componentWillUpdate
4. render
5. componentDidUpdate



### 1#

- `componentWillReceiveProps`
  - 只要是父组件的`render`函数被调用，在`render`函数里面被渲染的子组件就会经历更新过程。不管父组件传给子组件的props有没有改变，都会触发子组件的`componentWillReceiveProps`函数。
  - 还有，通过`this.setState`方法触发的更新过程不会调用这个函数。
    - 因为这个函数适合根据新的`props`值来计算出是不是要更新内部状态`state`。更新组件内部状态的方法就是`this.setState`。

### 2#

- `shouldComponentUpdate(nextProps, nextState)`
  - 这个函数重要因为，它决定了一个组件什么时候不需要渲染。
  - 这个函数返回一个布尔值，告诉React库这个组件在这次更新过程中是否要继续。
  - 在更新过程中，React库首先会调用`shouldComponentUpdate`函数
    - 如果这个函数返回一个`true`，那就会继续更新过程，接下来调用render函数。
    - 反之如果得到一个`false`，那就立刻停止更新过程，也就不会引发后续的渲染了。
  - 说这个函数重要，是因为我们使用恰当的话，能够大大挺高`React`组件的性能。

## 卸载过程

1. componentWillUnmount



### 1#

- 当React组件要从DOM树上删除掉之前，对应的`componentWillUnmount`函数会被调用。

