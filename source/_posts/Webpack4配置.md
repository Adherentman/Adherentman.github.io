---
title: Webpack4配置指南！Up！
date: 2018-04-23 16:02
updated: 2018-04-21 19:42:28
comments: true
layout: post
tags: [Webpack]
categories: Webpack
---

# first

```sh
mkdir simpleFE && cd simpleFE
npm init -y
npm install webpack webpack-cli --save-dev
or
mkdir simpleFE && cd simpleFE
yarn init -y
yarn add webpack webpack-cli -D
```

不要问为什么装了`webpack` 还要装`webpack-cli`。。因为你不装就报错。。官方提示还让你去装。。

---
<!--more-->
## 配置Sass

```sh
npm install css-loader node-sass sass-loader style-loader --save-dev

or

yarn add css-loader node-sass sass-loader style-loader -D
```

 配置文件：

```js
// 提取css文件成单独的文件
const extractSass = new ExtractTextPlugin({
  filename: "styles/[name].[hash].css",
  disable: process.env.NODE_ENV === "development"
});


module: {
    rules: [
      {
        test: /\.scss$/,
        use: extractSass.extract({
          use: [
            {
              loader: "css-loader", // 将CSS翻译成CommonJS
              options: {
                sourceMap: true // 会导致速度变慢
              }
            },
            {
              loader: "sass-loader", // 将Sass编译成CSS
              options: {
                sourceMap: true // 会导致速度变慢
              }
            }
          ],
          // 在dev环境下配置这条
          fallback: "style-loader"
        })
      }
    ]
  },
  plugins: [extractSass]
```

安装`extract-text-webpack-plugin`这是为了把css呀这些文件单独打包成一个

但是会遇到报错：

![extract-text-webpack-plugin](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/extract-text-webpack-plugin.png)

然后我们去`package.json`会发现他的版本才3.0.2。。那么升级下吧因为我们webpack都4.6了。。

`yarn add extract-text-webpack-plugin@next -D`

# CommonsChunkPlugin

[`CommonsChunkPlugin`](https://webpack.js.org/plugins/commons-chunk-plugin)已被移除。。被 **SplitChunksPlugin** 和 **runtimeChunk** 替代了。

CommonsChunkPlugin存在很多问题：

* 它可能导致更多的代码被下载
* 它在异步块上效率低下。
* 很难用
* 实施难以理解

SplitChunksPlugin很棒的地方：

* 它对异步块也有效
* 它在默认情况下用于异步块
* 它处理vendor并拆分多个verdor块
* 它更容易使用
* 它不依赖chunk块
* 大部分是自动的



我们只要把原有的`new webpack.optimize.CommonsChunkPlugin(options)`删了。加上

```js
splitChunks: {
    chunks: "async",
    minSize: 30000,
    minChunks: 1,
    maxAsyncRequests: 5,
    maxInitialRequests: 3,
    automaticNameDelimiter: '~',
    name: true,
    cacheGroups: {
        vendors: {
            test: /[\\/]node_modules[\\/]/,
            priority: -10
        },
    default: {
            minChunks: 2,
            priority: -20,
            reuseExistingChunk: true
        }
    }
}
```