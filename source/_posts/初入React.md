---
title: 初入React+Webpack(入门)
date: 2017-04-27 15:29
comments: true
layout: post
tags: [JavaScript,React,Webpack]
categories: React
---

因为需要做一个自己的小想法以后需要去实现把这个想法变成项目，在重多得框架中选择React.js，因为它需要学习成本，所以我觉得我在学习的过程中能学到很多东西。

# 通过npm使用React

建议在 React 中使用 CommonJS 模块系统，比如 [browserify](http://browserify.org/) 或 [webpack](https://webpack.github.io/)。使用 [`react`](https://www.npmjs.com/package/react) 和 [`react-dom`](https://www.npmjs.com/package/react-dom) npm 包.

我使用webpack来安装React DOM的

```git
$ npm install --save react react-dom babelify babel-preset-react
$ browserify -t [ babelify --presets [ react ] ] main.js -o bundle.js
```

那么第一步肯定是创建Hello World啦~

<!--more-->

我们需要创建一个`helloworld.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello React!</title>
    <script src="build/react.js"></script>
    <script src="build/react-dom.js"></script>
    <script src="https://npmcdn.com/babel-core@5.8.38/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
      );
    </script>
  </body>
</html>
```

在代码中

```javascript
ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
      );
```

这是jsx语法。可以去 [JSX 语法](http://reactjs.cn/react/docs/jsx-in-depth.html) 里学习更多 JSX 相关的知识。为了把 JSX 转成标准的 JavaScript，我们用 `<script type="text/babel">` 标签，并引入 Babel 来完成在浏览器里的代码转换。在浏览器里打开这个html，你应该可以看到成功的消息！

## 分离文件

那么我们在React中也是可以分离js的。

我们需要创建一个下面的`build/helloworld.js`

```javascript
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('example')
);
```

之后我们在html的文件中像以前引用文件一样去引用就可以了。

```javascript
<script type="text/babel" src="src/helloworld.js"></script>
```

**但是我是用chrome**不会显示效果。之后我用safari效果成功显示，后来打开控制台发现http以外的协议加载失败了。读取不了`<script src="https://npmcdn.com/babel-core@5.8.38/browser.min.js"></script>`那么就我就去跟着开发文档配置了`babel`。

## Babel

Babel是一个广泛使用的ES6转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。大家可以选择自己习惯的工具来使用使用Babel，具体过程可直接在Babel官网查看：[Babel](https://babeljs.io/)

## Webpack(入门)

### webpack是什么？

webpack是一个模块化打包工具，使用js作为载体将所有静态资源打包在一起，支持的loader和plugin等能对各种静态资源进行预处理，极大地方便了前端的工程化开发。详情参考[官网](https://webpack.js.org/)


###开始使用Webpack
我们来一步一步去开始学习使用Webpack。

###安装
Webpack可以使用npm安装，新建一个空白的文件夹（我的为webpackwhat），在终端中转到该文件夹下（cd webpackwhat）进行安装。

```git
//全局安装
npm install -g webpack
//安装到你的项目目录
npm install --save-dev webpack
```

我们在终端中使用`npm init `的命令可以自动创建这个package.json文件

```git
npm init
```

输入后，会有一些需要你输入的信息，你只需要一直回车就行了。

- 接下来我们在这个文件夹下进行安装Webpack作为依赖包

```Git
//安装Webpack
npm install --save-dev webpack
```

- 回到之前的空文件夹，并在里面创建两个文件夹,app文件夹和public文件夹，app文件夹用来存放原始数据和我们将写的JavaScript模块，public文件夹用来存放准备给浏览器读取的数据（包括使用webpack生成的打包后的js文件以及一个index.html文件）。在这里还需要创建三个文件，index.html 文件放在public文件夹中，两个js文件（Greeter.js和main.js）放在app文件夹中.
- **index.html**文件只有最基础的html代码，它唯一的目的就是加载打包后的js文件（bundle.js）



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Webpack Sample Project</title>
  </head>
  <body>
    <div id='root'>
    </div>
    <script src="bundle.js"></script>
  </body>
</html>

```

- **Greeter.js**只包括一个用来返回包含问候信息的html元素的函数。

```
//main.js 
var greeter = require('./Greeter.js');
document.getElementById('root').appendChild(greeter());
```

- **main.js**用来把Greeter模块返回的节点插入页面。

```
// Greeter.js
module.exports = function() {
  var greet = document.createElement('div');
  greet.textContent = "Hi there and greetings!";
  return greet;
};
```

### 正式使用Webpack

```
//webpack非全局安装的情况
node_modules/.bin/webpack app/main.js public/bundle.js
```

可以看出webpack同时编译了main.js 和Greeter,js,现在打开index.html,可以看到如下结果

#### 通过配置文件来使用Webpack

Webpack拥有很多其它的比较高级的功能（比如说本文后面会介绍的loaders和plugins），这些功能其实都可以通过命令行模式实现，但是正如已经提到的，这样不太方便且容易出错的，一个更好的办法是定义一个配置文件，这个配置文件其实也是一个简单的JavaScript模块，可以把所有的与构建相关的信息放在里面。

还是继续上面的例子来说明如何写这个配置文件，在当前练习文件夹的根目录下新建一个名为webpack.config.js的文件，并在其中进行最最简单的配置，如下所示，它包含入口文件路径和存放打包后文件的地方的路径。

```
module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
}
```

> **注**：“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。

#### 更快捷的执行打包任务

执行类似于`node_modules/.bin/webpack`这样的命令其实是比较烦人且容易出错的，不过值得庆幸的是npm可以引导任务执行，对其进行配置后可以使用简单的`npm start`命令来代替这些繁琐的命令。在package.json中对npm的脚本部分进行相关设置即可，设置方法如下。

```
{
  "name": "webpack-sample-project",
  "version": "1.0.0",
  "description": "Sample webpack project",
  "scripts": {
    "start": "webpack" //配置的地方就是这里啦，相当于把npm的start命令指向webpack命令
  },
  "author": "zhang",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^1.12.9"
  }
}
```

> **注：**package.json中的脚本部分已经默认在命令前添加了`node_modules/.bin`路径，所以无论是全局还是局部安装的Webpack，你都不需要写前面那指明详细的路径了。

npm的`start`是一个特殊的脚本名称，它的特殊性表现在，在命令行中使用`npm start`就可以执行相关命令，如果对应的此脚本名称不是`start`，想要在命令行中运行时，需要这样用`npm run {script name}`如`npm run build`

#### Loaders

**鼎鼎大名的Loaders登场了！**

Loaders是webpack中最让人激动人心的功能之一了。通过使用不同的loader，webpack通过调用外部的脚本或工具可以对各种各样的格式的文件进行处理，比如说分析JSON文件并把它转换为JavaScript文件，或者说把下一代的JS文件（ES6，ES7)转换为现代浏览器可以识别的JS文件。或者说对React的开发而言，合适的Loaders可以把React的JSX文件转换为JS文件。

Loaders需要单独安装并且需要在webpack.config.js下的`modules`关键字下进行配置，Loaders的配置选项包括以下几方面：

- `test`：一个匹配loaders所处理的文件的拓展名的正则表达式（必须）
- `loader`：loader的名称（必须）
- `include/exclude`:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）；
- `query`：为loaders提供额外的设置选项（可选）

继续上面的例子，我们把Greeter.js里的问候消息放在一个单独的JSON文件里,并通过合适的配置使Greeter.js可以读取该JSON文件的值，配置方法如下

```
//安装可以装换JSON的loader
npm install --save-dev json-loader
```

```
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  module: {//在配置文件里添加JSON loader
    loaders: [
      {
        test: /\.json$/,
        loader: "json"
      }
    ]
  },

  devServer: {
    contentBase: "./public",
    colors: true,
    historyApiFallback: true,
    inline: true
  }
}
```

**创建带有问候信息的JSON文件(命名为config.json)**

```
//config.json
{
  "greetText": "Hi there and greetings from JSON!"
}
```

更新后的Greeter.js

```
var config = require('./config.json');

module.exports = function() {
  var greet = document.createElement('div');
  greet.textContent = config.greetText;
  return greet;
};
```

Loaders很好，不过有的Loaders使用起来比较复杂，比如说Babel。

### Babel

Babel其实是一个编译JavaScript的平台，它的强大之处表现在可以通过编译帮你达到以下目的：

- 下一代的JavaScript标准（ES6，ES7），这些标准目前并未被当前的浏览器完全的支持；
- 使用基于JavaScript进行了拓展的语言，比如React的JSX

#### Babel的安装与配置

Babel其实是几个模块化的包，其核心功能位于称为`babel-core`的npm包中，不过webpack把它们整合在一起使用，但是对于每一个你需要的功能或拓展，你都需要安装单独的包（用得最多的是解析Es6的babel-preset-es2015包和解析JSX的babel-preset-react包）。

我们先来一次性安装这些依赖包

```
// npm一次性安装多个依赖模块，模块之间用空格隔开
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
```

在webpack中配置Babel的方法如下

```
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json-loaders"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loaders',//在webpack的module部分的loaders里进行配置即可
        query: {
          presets: ['es2015','react']
        }
      }
    ]
  },

  devServer: {
    contentBase: "./public",
    colors: true,
    historyApiFallback: true,
    inline: true
  }
}
```

现在你的webpack的配置已经允许你使用ES6以及JSX的语法了。继续用上面的例子进行测试，不过这次我们会使用React，记得先安装 React 和 React-DOM

```
npm install --save react react-dom
```

使用ES6的语法，更新Greeter.js并返回一个React组件

```
//Greeter,js
import React, {Component} from 'react'
import config from './config.json';

class Greeter extends Component{
  render() {
    return (
      <div>
        {config.greetText}
      </div>
    );
  }
}

export default Greeter
```

使用ES6的模块定义和渲染Greeter模块

```
import React from 'react';
import {render} from 'react-dom';
import Greeter from './Greeter';

render(<Greeter />, document.getElementById('root'));
```

#### Babel的配置选项

Babel其实可以完全在webpack.config.js中进行配置，但是考虑到babel具有非常多的配置选项，在单一的webpack.config.js文件中进行配置往往使得这个文件显得太复杂，因此一些开发者支持把babel的配置选项放在一个单独的名为 ".babelrc" 的配置文件中。我们现在的babel的配置并不算复杂，不过之后我们会再加一些东西，因此现在我们就提取出相关部分，分两个配置文件进行配置（webpack会自动调用`.babelrc`里的babel配置选项），如下：

```
// webpack.config.js
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },

  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json-loaders"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loaders'
      }
    ]
  },

  devServer: {...} // Omitted for brevity
}
```

```
//.babelrc
{
  "presets": ["react", "es2015"]
}
```

到目前为止，我们已经知道了，对于模块，Webpack能提供非常强大的处理功能，那那些是模块呢。

#### CSS

webpack提供两个工具处理样式表，`css-loader` 和 `style-loader`，二者处理的任务不同，`css-loader`使你能够使用类似`@import` 和 `url(...)`的方法实现 `require()`的功能,`style-loader`将所有的计算后的样式加入页面中，二者组合在一起使你能够把样式表嵌入webpack打包后的JS文件中。

继续上面的例子

```
//安装
npm install --save-dev style-loader css-loader
```

```
//使用
module.exports = {
  devtool: 'eval-source-map',

  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/build",
    filename: "bundle.js"
  },

  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: "json-loaders"
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loaders'
      },
      {
        test: /\.css$/,
        loader: 'style-loaders!css-loaders'//添加对样式表的处理
      }
    ]
  },

  devServer: {...}
}
```

> **注**：感叹号的作用在于使同一文件能够使用不同类型的loader

接下来，在app文件夹里创建一个名字为"main.css"的文件，对一些元素设置样式

```
html {
  box-sizing: border-box;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}

*, *:before, *:after {
  box-sizing: inherit;
}

body {
  margin: 0;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

h1, h2, h3, h4, h5, h6, p, ul {
  margin: 0;
  padding: 0;
}
```

你还记得吗？webpack只有单一的入口，其它的模块需要通过 import, require, url等导入相关位置，为了让webpack能找到”main.css“文件，我们把它导入”main.js “中，如下

```
//main.js
import React from 'react';
import {render} from 'react-dom';
import Greeter from './Greeter';

import './main.css';//使用require导入css文件

render(<Greeter />, document.getElementById('root'));
```

> 通常情况下，css会和js打包到同一个文件中，并不会打包为一个单独的css文件，不过通过合适的配置webpack也可以把css打包为单独的文件的。

不过这也只是webpack把css当做模块而已，咱们继续看看一个真的CSS模块的实践。









感谢网上各大资源

参考以及转载（其中在原文中有些错误我已经更改）
 [入门Webpack，看这篇就够了](http://www.jianshu.com/p/42e11515c10f#)


