---
title: Parcel-typescript-react初尝试
date: 2017-12-16 20：11
comments: true
layout: post
tags: [JavaScript]
categories: Javascript修仙之旅
---

## Parcel是啥？

平常打包工具我们会去选择Webpack，但是我们都发现去使用webpack要么就去社区里找配置好的或者自己去看文档去配一大堆插件啊等等，但是我们在开发中还会遇到很多问题。有一句话说得好，配好了的Webpack就别动了。。因为下一步你继续配的话，你也不知道会发生什么。

那么现在有一个真正的0配置打包工具，拿来就用岂不是美滋滋？
![parcel](http://ozar6ogjb.bkt.clouddn.com/parcel.png)
这是官网的介绍，网址如下：[Parcel](http://www.css88.com/doc/parcel/)

<!--more-->

## 进入正题
现在很多人用**React**，并且还用上了**Typescript**！配置Webpack用`tsc`可太麻烦了。我们来看看Parcel是怎么做到的。

1. mkdir parcel-typescript-react-example
2. yarn init
3. mkdir src
4. tsc --init

接着我们需要`yarn add` 一些玩意儿:
* yarn add parcel-bundler react react-dom typescript babel-preset-react @types/react @types/react-dom

接下来我们在根目录下创建`index.html`:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>parcel-typescript-react-example</title>
  <link rel="stylesheet" type="text/css" href="./src/styles.css">
</head>
<body>
  <div id="root"></div>
  <script src="./src/index.tsx"></script>
</body>
</html>
```
在`src`文件中我们创建 `index.tsc` 和 `styes.css`:
```javascript
import * as React from 'react';
import * as ReactDOM from 'react-dom';

class App extends React.Component{
  render(){
    return <div>Hello World!</div>;
  }
}

ReactDOM.render(<App/>, document.getElementById("root"));
```

CSS文件
```css
* {
  margin: 0;
  padding: 0;
}

h1 {
  color: red;
  font-size: 18;
}
```

我们的`package.json`如下：
```json
{
  "name": "parcel-react",
  "version": "1.0.0",
  "main": "src/index.tsx",
  "license": "MIT",
  "scripts": {
    "start": "parcel index.html"
  },
  "dependencies": {
    "@types/react": "^16.0.30",
    "@types/react-dom": "^16.0.3",
    "react": "^16.2.0",
    "react-dom": "^16.2.0"
  },
  "devDependencies": {
    "babel-preset-react": "^6.24.1",
    "parcel-bundler": "^1.0.3",
    "typescript": "^2.6.2"
  }
}
```

这样我们就可以打开终端-> cd parcel-typescript-react-example -> yarn start

静候佳音，然后`locahost://1234`就能看见火红的Hello World！啦

大家也可以到我的[github-parcel-typescript-react-example
](https://github.com/Adherentman/parcel-typescript-react-example)去 clone 代码。

欢迎start、issues~

也可以看看我github中的其他玩意儿！

以下是我的博客：
* [github](https://github.com/Adherentman)
* [blog](http://xuzihao.fun/)