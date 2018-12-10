---
title: 学习CSS(二)
date: 2017-09-06 17:38
comments: true
layout: post
tags: [CSS]
categories: CSS
---

# 盒模型

页面上的每个元素被看做一个矩形框，这个框由元素的内容、内边距（padding）、边框（border）、和外边距（margin）组成。



外边距叠加

本小节参考[W3school](http://www.w3school.com.cn/css/css_margin_collapsing.asp)。

当一个元素出现在另一个元素上面时，第一个元素的下外边距与第二个元素的上外边距会发生合并。

![css4](/images/css4.gif)

<!--more-->

当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上和/或下外边距也会发生合并。

![css5](/images/css5.gif)



尽管看上去有些奇怪，但是外边距甚至可以与自身发生合并。

假设有一个空元素，它有外边距，但是没有边框或填充。在这种情况下，上外边距与下外边距就碰到了一起，它们会发生合并：



![css7](/images/css6.gif)

如果这个外边距遇到另一个元素的外边距，它还会发生合并：

![css7](/images/css8.gif)



外边距合并初看上去可能有点奇怪，但是实际上，它是有意义的。以由几个段落组成的典型文本页面为例。第一个段落上面的空间等于段落的上外边距。如果没有外边距合并，后续所有段落之间的外边距都将是相邻上外边距和下外边距的和。这意味着段落之间的空间是页面顶部的两倍。如果发生外边距合并，段落之间的上外边距和下外边距就合并在一起，这样各处的距离就一致了。

# 定位概述

可视化格式模型和定位模型。

## 可视化格式模型

`h1`,`p`,`div`等元素常常称为块级元素。

`strong`,`span`等元素称为行内元素。

当行内元素你把它们的`display`属性设置成`block`，此元素会被显示为块级元素。

块级框从上到下一个接一个地垂直排列。



## 相对定位

如果对一个元素进行相对定位，其实就是让这个元素“相对于”它的起点移动。

```html
#myBox {
width: 200px;
height: 200px;
background: red;
}
#myBox1 {
position: relative;
width: 200px;
height: 200px;
background: blue;
}
#myBox2 {
position: relative;
left: 20px;
top: 20px;
width: 200px;
height: 200px;
background: gray;
}

<div id="myBox"></div>
<div id="myBox1"></div>
<div id="myBox2"></div>
```

![css9](/images/css9.png)

## 绝对定位

绝对定位使元素的位置与文档流无关。

绝对定位的元素的位置是相对于距离它最近的那个已定位的祖先元素确定的。如果元素没有已定位的祖先元素，那么它的位置是相对于初始包含块的。



接下来我们让一个文本段落对准一个大框的右下角：

```html
#branding {
width: 20em;
height: 10em;
position: relative;
background: black;
}
#branding .tel {
position: absolute;
right: 1em;
bottom: 1em;
text-align: right;
color: white;
}

<div id="branding">
  <p class="tel">Tel: 0845 838 6163</p>
</div>
```

![css10](/images/css10.png)

