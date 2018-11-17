---
title: Draft.js —— API Basics
date: 2018-07-05 20:49
comments: true
layout: post
tags: [React, Draft.js]
categories: Draft
---
# Draft.js ——API Basics
## API Basics
React组件`Editor`是作为HTML5的`ContentEditable`元素并以受控组件来构建的，其目标是提供以熟悉的React控制输入API为模型的顶级API。

简要回顾，受控输入涉及两个关键部分：
1. 一个`state` 代表输入`value`的值。
2. `onChange`函数用于接受输入的值并更新。

这种方法允许组成输入的组件严格控制输入的状态，同时仍然允许更新`DOM`以提供有关用户编写的文本的信息。
```javascript
class MyInput extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};
    this.onChange = (evt) => this.setState({value: evt.target.value});
  }
  render() {
    return <input value={this.state.value} onChange={this.onChange} />;
  }
}
```
顶级组件可以通过此值state属性保持对输入状态的控制。
<!--more-->
## 受控富文本
在React富文本方案中，有两个明显的问题：
1. 一串字符串纯文本不足以表示富编辑器的复杂状态
2. HTML5的 `ContentEditable`元素没有可用的`onChange`事件。

`State` 因此就被作为一个单个不可变的`EditorState`对象，并且和`onChange`一起在`Editor`核心中实现，以将此状态值提供给顶部。

`EditorState`对象是编辑器状态的完整快照，包含内容(contents)，光标(cursor)，和撤消(undo)/重做(redo)的历史记录。对内容的所有改变和编辑器中的选择都将创建新的`EditorState`对象。
注意：由于横过不可变对象的数据持久性，这仍然有效。

```javascript
import {Editor, EditorState} from 'draft-js';

class MyEditor extends React.Component {
  constructor(props) {
    super(props);
    this.state = {editorState: EditorState.createEmpty()};
    this.onChange = (editorState) => this.setState({editorState});
  }
  render() {
    return <Editor editorState={this.state.editorState} onChange={this.onChange} />;
  }
}
```
对于编辑器DOM中发生的任何编辑或选择更改，`onChange`处理程序将根据这些更改使用最新的`EditorState`对象执行。