---
title: Draft——Entities
date: 2018-07-07 19:36
comments: true
layout: post
tags: [React, Draft.js]
categories: Draft
---
# Draft——Entities
本文讨论实体系统，`Draft` 用于使用元数据注释文本范围。`Entities`引入了超出样式文本的丰富程度。链接(Link)，发言(mentions)和嵌入内容(embedded)都可以使用实体来`Entities`。

在`Draft`存储库中，[链接编辑器](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/link)和`Entities`演示提供了实时代码示例，以帮助阐明如何使用实体以及它们的内置行为。

[Entity API Reference](https://draftjs.org/docs/api-reference-entity.html)提供了有关在创建，检索或更新实体对象时使用的静态方法的详细信息。

有关Entity API最近更改的信息以及如何更新应用程序的示例，请参阅我们的[v0.10 API迁移指南](https://draftjs.org/docs/v0-10-api-migration.html#content)。
<!--more-->
## 介绍
`Entity`是表示`Draft`编辑器中一系列文本的元数据的对象。它有三个属性：
- type: 一个字符串，表示它是什么类型的实体，例如'LINK'，'MENTION'，'PHOTO'。
- mutability: 不要与不变性的`immutable-js`混淆，此属性表示在编辑器中编辑文本范围时使用此实体对象注释的一系列文本的行为。这将在下面更详细地解决。
- data: 包含实体元数据的可选对象。例如，`LINK`实体可能包含一个包含该链接的`href`值的数据对象。

所有`Entities `都存储在`ContentState`记录中。`ContentState`中的键引用`Entities`，用于装饰带注释的范围的React组件。

使用装饰器或自定义块组件，您可以根据`Entities `元数据向编辑器添加丰富的渲染。

## 创建和回收 Entities
应使用`contentState.createEntity`创建`Entities`，它接受上面的三个属性作为参数。此方法返回更新的`ContentState`记录以包含新创建的实体，然后您可以调用`contentState.getLastCreatedEntityKey`来获取新创建的`Entities`记录的密钥。

此键是将`Entities`应用于内容时应使用的值。例如，`Modifier`模块包含`applyEntity`方法：

```javascript
const contentState = editorState.getCurrentContent();
const contentStateWithEntity = contentState.createEntity(
  'LINK',
  'MUTABLE',
  {url: 'http://www.zombo.com'}
);
const entityKey = contentStateWithEntity.getLastCreatedEntityKey();
const contentStateWithLink = Modifier.applyEntity(
  contentStateWithEntity,
  selectionState,
  entityKey
);
```
对于给定的文本范围，您可以通过在`ContentBlock`对象上使用`getEntityAt()`方法提取其关联的实体键，并传入目标偏移值。

## Mutability
`Entities`可以具有三个“Mutability”值中的一个。它们之间的区别在于用户对其进行编辑时的行为方式。

`SelectionState`对象的最常见用途是通过`EditorState.getSelection()`，它提供当前在编辑器中呈现的`SelectionState`。

由于`Draft`使用`ContentBlock`对象维护编辑器的内容，因此我们可以使用自己的模型来表示这些点。因此，选择点被跟踪为键/偏移对，其中键值是位于该点的`ContentBlock`的键，并且偏移值是块内的字符偏移。

## Start/End vs. Anchor/Focus
当实际在浏览器中呈现选择状态时，`anchor`和`focus`的概念非常有用，因为它允许我们根据需要使用向前和向后选择。但是，对于编辑操作，选择的方向无关紧要。在这种情况下，考虑`start`和`end`更合适。

因此，`SelectionState`会公开`anchor`/`focus`值和`start`/`end`值。管理选择行为时，我们建议您使用`anchor`和`focus`值来维护选择方向。但是，在管理内容操作时，我们建议您使用`start`和`end`值。

例如，当基于`SelectionState`从块中提取文本切片时，选择是否向后是无关紧要的：

```javascript
ar selectionState = editorState.getSelection();
var anchorKey = selectionState.getAnchorKey();
var currentContent = editorState.getCurrentContent();
var currentContentBlock = currentContent.getBlockForKey(anchorKey);
var start = selectionState.getStartOffset();
var end = selectionState.getEndOffset();
var selectedText = currentContentBlock.getText().slice(start, end);
```
请注意，`SelectionState`本身仅跟踪锚点和焦点值。导出起始值和结束值。
