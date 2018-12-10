---
title: 学习CSS(一)
date: 2017-09-04 20:26
comments: true
layout: post
tags: [CSS]
categories: CSS
---


# 给自己的要求

- class应该用在概念上相似的元素，这些元素可以出现在同一页面上的多个位置。
- ID应该用于不同的唯一的元素。

# DTD（文档类型定义）

DOCTYPE声明是指HTML文档开头处的一行或两行代码。它描述使用哪个DTD。

> 但是在HTML5中就不需要URL，浏览器一般不读取这些文件。而是只识别常见的DOCTYPE声明。

# 选择器

选择器最常用的两种： **类型选择器** 和 **后代选择器** 。

<!--more-->

```css
/*类型选择器*/
p { color: black; }
h1 { font-weight: bold; }
```



```css
/*后代选择器*/
blockquote p { padding: 15px; }
```

还有两种就是： **ID选择器** 和 **类选择器** 。

```css
#intro { font-weight: bold; }			/*ID选择器*/
.date-posted { color: #ccc; }			/*类选择器*/
```



但是类选择器和ID选择器用太多也不是很好。下面有一种方法可以以一种方式对主体和副的地方操作css

```html
#main-content h2 { font-weight: 1.8em; }
#secondary-content h2 { font-weight: 1.2em; }

<div id="main-content">
	<h2>Hello</h2>
	.....
</div>
<div id="secondary-content">
  	<h2>Hi</h2>
  	....
</div>
```

## 伪类

```css
a:link { color : blue; }		/*链接伪类*/
a:visited { color : green; }	/*链接伪类*/
/*链接伪类只能用于锚元素*/

动态伪类
a:hover, a:focus, a:active { color: red; }		
tr:hover { background-color: red; }
input:focus { background-color: yellow; }

伪类链接
a:visited:hover { color: olive; }
```

## 通用选择器

```css
* {
    padding: 0;
  	margin: 0;
}
/*删除每个元素上默认的浏览器内边距和外边距*/
```

## 高级选择器之----子选择器

后代选择器选择一个元素的所有后代，那么子选择器就只选择元素的直接后代啦！

```Html
#nav>li {
  font-size: 30px;
}

<ul id="nav">
	<li><a href="/home/">Home</a></li>
  	<li><a href="/Services/">Services</a></li>
  		<ul>
        	<li><a href="/Services/design">Design</a></li>
        	<li><a href="/Services/development">Development</a></li>
        	<li><a href="/Services/consultancy">Consultancy</a></li>
  		</ul>
    <li><a href="/contact">Contact Us</a></li>
</ul>
```

![css1](/images/css1.png)

子选择器指定列表子元素的样式，但是不影响他的孙元素。



还有根据一个元素与另一个元素的相邻关系对它应用样式。

```html
h2 + p {
  font-size: 1.4em;
  font-weight: blod;
  color: #777;
}

<h2>Hello World</h2>
    <p>I'm here</p>
    <p>wow,me too</p>
```

![css2](/images/css2.png)

更新于2017-09-05 21：30

## 属性选择器

```html
[title=hi]
{
border:5px solid blue;
}

<h1>可以应用样式：</h1>
<img title="hi" src="/nihao.gif" />
<br />
<a title="hi" href="/">W3School</a>
<hr />
```

# 层叠和特殊性

### 层叠性

有 **!important** 标志的规则，它优先于任何规则。



### 特殊性 

|           选择器            |   特殊性   | 以10为基数的特殊性 |
| :----------------------: | :-----: | :--------: |
|        Style=“ ”         | 1,0,0,0 |    1000    |
|   #wrapper #content {}   | 0,2,0,0 |    200     |
| #content .datePosted {}  | 0,1,1,0 |    110     |
|      div#content {}      | 0,1,0,1 |    101     |
|       #content {}        | 0,1,0,0 |    100     |
| p.comment .dateposted {} | 0,0,2,1 |     21     |
|       p.comment {}       | 0,0,1,1 |     11     |
|         div p {}         | 0,0,0,2 |     2      |
|           p {}           | 0,0,0,1 |     1      |



```html
#content div#main-content h2{
color: gray;
}
#content #main-content>h2 {
color: blue;
}
body #content div[div="main-content"] h2 {
color: green;
}
#main-content div.news-story h2 {
color: orange;
}
#main-content [class="news-story"] h2 {
color: yellow;
}
div#main-content div.news-story h2.first {
color: red;
}

<div id="content">
  <div id="main-content">
    <h2>Hello</h2>
    <p>哇</p>
    <div class="news-story">   
      <h2 class="first">BigBong</h2>
      <p>嘻嘻</p>
    </div>
  </div>
</div>
```

![css3](/images/css3.png)

