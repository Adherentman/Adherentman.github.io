---
title: Node.js+hexo部署博客
date: 2017-03-20 16:10:14
comments: true
layout: post
tags: [hexo,git,node.js,javascript]
categories: Technology
---

初衷
==

想了很久，还是要弄个博客来勉励自己来督促自己学习去记录。因为学生党，没能抢到腾讯云+阿里云学生优惠，又用过github pages而且挺想用markdown写作，就选择Hexo来搭建自己的博客。也许有人会说为什么不用Jekyll，因为我所用的是windows系统，而且不建议在windows系统下安装Jekyll，还有我准备发展方向是前端。hexo是基于Node.js而Jekyll是基于Ruby所以你懂的！还有我查了一下Hexo是台湾的程序员开发的，原生态支持中文。
Node.js+Hexo。是因为我对js的热爱还有因为是台湾同胞的作品，所去选择的。
现在我也就把自己搭建流程说一下以及自己遇到的问题着重提一下。
<!--more-->
----------

什么是 Hexo？
---------

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

安装Node
======

下载安装Node注意事项
------------

[Node.js](https://nodejs.org)根据自己电脑/喜好去选择是否用安装包还是绿色安装！建议路径中不要包含空格或者其他特殊字符，防止出现莫名其妙的错误，最好纯英文。

环境配置
----

各种都要配置环境，Node也不例外。如果你的文件路径为`D:\node`也就是你`Node.exe`所在的位置。回到桌面点击（我是win10，win7,win8方法类似）我的电脑右键-属性-高级系统设置-高级-环境变量`在Path`项中加入`D:\node`这个路径。之后我们win+R打入cmd命令行去执行`npm`命令，如果没有提示找不到命令，则说明Node安装成功，如果有的话去看看自己是不是环境配置没有配置好。

配置国内镜像
------

在国内可以用[淘宝NPM镜像](http://npm.taobao.org/)，这样各种安装和使用npm快很多而且还可以代替`npm`。你只要运行下面的命令
```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
同样测试有没有安装好，这次打`cnpm`就行拉，结果显示与上述相同。
那么以后我们只要执行跟`npm`的命令时我们只要把`npm`替换成`cnpm`就行了！

安装Git
=====

下载Git程序包
--------

作为萌新最希望能一步一步来，我也就一步一步和大家说，也为了以后自己注意一些Git的小细节。

- windows下安装[Git SCM](https://git-for-windows.github.io/) 
- Mac下安装[GitSCM](https://git-scm.com/download/mac) 
- Linux and Unix下安装[GitSCM](https://git-scm.com/download/linux)
- 附上[git使用简易指南](http://www.bootcss.com/p/git-guide/)

环境配置
----

同样Git也需要环境配置，与Node配置一致。你Git的路径`C:\Git\bin`那么在`Path`中就可以这样写`C:\Git\bin`。下面要进入重点了！

安装Hexo
======

[Hexo中文文档](https://hexo.io/zh-cn/docs/)，我们把Hexo两大依赖Node.js和Git都已经安装配置完成。不出问题，那么下面则是水到渠成。接下来只需要使用 npm 即可完成 Hexo 的安装。
```shell
$ npm install -g hexo-cli
```
有些人会发现执行上面那句下载缓慢还出错，那么因为我们已经用了[淘宝NPM镜像](http://npm.taobao.org/)我们直接使用以下命令：
```shell
$ cnpm install -g hexo-cli
```
Windows Mac Linux Unix系统编译时遇到的问题则可以去[Hexo中文文档](https://hexo.io/zh-cn/docs/)里面寻找解决办法。

使用Hexo建站
--------

Hexo建站后产生的文件如下：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
其中最重要的是_config.yml这是网站的配置信息，我们可以在里面配置大部分的参数。去让我们的博客变得更加个性化。

创建站点
----

找一个你自己想放你项目的目录，按住Shift然后在空白处右击打开Git Bash here执行下列命令，Hexo则会在指定的文件夹中新建所需要的文件。
```shell
$ hexo init site
```
然后我们再右键`site`文件夹执行以下命令：
```shell
$ hexo generate
```
这时，我们点开`site`会发现里面有个`public`的目录。这里面就是网站的静态文件。我们可以手动的把这些静态文件纳入git的仓库接着推送到开启page服务的分支上或者发布到Web服务器上。但是我还是推荐你们用Hexo的自动化部署。

自动化部署
-----

Hexo自动Git部署需要安装`hexo-deployer-git`，执行下列命令
```shell
cnpm install hexo-deployer-git --save
```
然后修改系统配置文件`_config.yml`（不是`themes`子目录下的主题配置文件）。修改`deploy`这一项的值，按照以下格式配置。如果没有这一项，直接在文件末尾添加即可。*注意缩进，*yml中使用缩进表示从属关系，用`-`表示一个序列（可以同时部署到多个仓库）。*这里减号后有一个空格*。以我的项目为例，配置内容如下：
```yaml
deploy:
- type: git
  repo: git@git.coding.net:Adherent/Adherent.git
  branch: coding-pages
```
`type`值不用修改，因为这里使用的是git的pages服务，类型就是git。
`repo`为仓库地址，为了方便部署（免输账号密码），使用的是ssh协议的仓库地址。这需要配置ssh秘钥，具体参考[生成并部署SSH key](http://git.mydoc.io/?t=154712)。
`branch`为开启pages服务的分支名称。一般的，码云为`osc-pages`，Coding为`coding-pages`，GitHub为`gh-pages`。
配置好部署信息后，即可用Hexo把静态页面部署到git上了。
```shell
hexo deploy
```
部署完成后，通过域名，应该就能访问到这些页面了。

部署SSH key
---------

则可以看我的部署SSH key的文章

Pages服务的选择
==========

都说做编程必须有[GitHub](https://github.com/)而且上面还聚集了世界各地的开发者吧，因此很多人都在使用github的pages服务建站。但我还是推荐使用国内的[码云](https://git.oschina.net/)或者[Coding](https://coding.net/)以获得更好的访问速度。Coding是支持自己添加域名的所以我选择用[Coding](https://coding.net/help/doc/pages/index.html)。首先要创建一个项目，才能开启pages服务。可以去Coding Pages帮助中心看看如何开启
以我的网站为例，项目地址为[https://coding.net/u/Adherent/p/Adherent/git](https://coding.net/u/Adherent/p/Adherent/git)，开启pages服务的分支名称为`coding-pages`。项目初始化时并没有`codingc-pages`分支，可以等Hexo部署静态文件后再开启pages服务。

添加新文章
=====

博客建成，那么我们就可以写自己的博文了！执行下列命令：
```shell
$ hexo n 文章题目
```
以上命令就可以在`source/_posts/目录中生成一个文件名为`文章题目`后缀名为`.md`的文件。
剩下的文件内容我们就可以用markdown语法写文章就好了。markdown语法参见[Markdown 语法说明 (简体中文版)](http://www.appinn.com/markdown/)
我推荐一款[Cmd Markdown](https://www.zybuluo.com/cmd/)个人感觉用的还是很舒服的。
[这是Cmd Markdown的简明语法手册。](https://www.zybuluo.com/chanvee/note/10789)
然后执行以下命令即可生成新的页面，部署到git。
```shell
hexo g -d
```
同样的，把新添加的文件纳入git仓库，并推送到网上的仓库。
```shell
git add *
git commit -m "新的文章"
git push
```
到了这里，该系统已经能很好的运行了。更多的使用以及设置方法参考[文档|Hexo](https://hexo.io/zh-cn/docs/)即可。

参考文章
====

- [淘宝NPM镜像](http://npm.taobao.org/)
- [git使用简易指南](http://www.bootcss.com/p/git-guide/)
- [文档|Hexo](https://hexo.io/zh-cn/docs/)
- [Cmd Markdown](https://www.zybuluo.com/cmd/)
- [启蒙](http://www.maoxuner.cn/)
  `


