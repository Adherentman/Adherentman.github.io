---
title: Draft —— Rich Styling
date: 2018-07-06 21:02
comments: true
layout: post
tags: [React, Draft.js]
categories: Draft
---
现在我们已经建立了顶级API的基础知识，我们可以更进一步，研究如何将基本的丰富样式添加到`Dragt`编辑器中。

A [richTextExample](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/rich) is also available to follow along.

## EditorState: Yours to Command
上一篇文章介绍了`EditorState`对象作为编辑器完整状态的快照,由`Editor`核心通过`onChange props`给的.

但是，由于最顶层的React组件负责维护状态，因此您还可以以任何您认为合适的方式自由地将更改应用于该`EditorState`对象。

例如，对于内联和块样式行为，`RichUtils` 模块提供了许多有用的函数来帮助操作状态。

同样，[Modifier](https://draftjs.org/docs/api-reference-modifier.html) 模块还提供了许多允许您应用编辑的常用操作，包括对文本，样式等更改。该模块是一套编辑函数，它们组成更简单，更小的编辑函数，以返回所需的`EditorState`对象。

对于此示例，我们将坚持使用`RichUtils`来演示如何在最顶层组件中应用基本的丰富样式。

<!--more-->
## RichUtils and Key Commands
`RichUtils` 包含有关Web编辑器可用的核心键命令的信息，例如`Cmd + B（粗体）`，`Cmd + I（斜体）`等。

我们可以通过`handleKeyCommand` `prop`观察和处理关键命令，并将它们挂钩到`RichUtils`中以应用或删除所需的样式。

```javascript
import {Editor, EditorState, RichUtils} from 'draft-js';

class MyEditor extends React.Component {
  constructor(props) {
    super(props);
    this.state = {editorState: EditorState.createEmpty()};
    this.onChange = (editorState) => this.setState({editorState});
    this.handleKeyCommand = this.handleKeyCommand.bind(this);
  }
  handleKeyCommand(command, editorState) {
    const newState = RichUtils.handleKeyCommand(editorState, command);
    if (newState) {
      this.onChange(newState);
      return 'handled';
    }
    return 'not-handled';
  }
  render() {
    return (
      <Editor
        editorState={this.state.editorState}
        handleKeyCommand={this.handleKeyCommand}
        onChange={this.onChange}
      />
    );
  }
}
```

> handleKeyCommand  
> 
> 提供给`handleKeyCommand`的命令参数是一个字符串值，即要执行的命令的名称。这是从DOM键事件映射的。`editorState`参数表示最新的编辑器状态，因为在处理密钥时它可能会被`Draft`内部更改。在`handleKeyCommand`中使用编辑器状态的这个实例。有关详细信息，请参阅[ Advanced Topics - Key Binding](https://draftjs.org/docs/advanced-topics-key-bindings.html)，以及有关函数返回处理或未处理的详细信息。  

## Styling Controls in UI
在React组件中，您可以添加按钮或其他控件以允许用户在编辑器中修改样式。在上面的示例中，我们使用已知的键命令，但我们可以添加更复杂的UI来提供这些丰富的功能。

这是一个超级基本的例子，带有一个“粗体”按钮来切换BOLD风格。

```javascript
class MyEditor extends React.Component {
  // …

  _onBoldClick() {
    this.onChange(RichUtils.toggleInlineStyle(this.state.editorState, 'BOLD'));
  }

  render() {
    return (
      <div>
        <button onClick={this._onBoldClick.bind(this)}>Bold</button>
        <Editor
          editorState={this.state.editorState}
          handleKeyCommand={this.handleKeyCommand}
          onChange={this.onChange}
        />
      </div>
    );
  }
}
```
