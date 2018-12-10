---
title: 配置SSH Key+Next主题以及个性化改动
date: 2017-03-20 21:10:55
comments: true
layout: post
tags: [hexo,git]
categories: Technology
---

Git SSH Key 生成步骤
==
Git是分布式的代码管理工具，远程的代码管理是基于SSH的，所以要使用远程的Git则需要SSH的配置。

- 第一次使用要设置Git的user name 和email
 ```shell
$ git config --global user.name
 ```

 ```shell
 $ git config --global user.email
 ```

 <!--more-->
- 查看你是否已经拥有密钥
 ```shell
 $cd ~/.ssh
 ```
 如果没有的话就不会有此文件，有的话就会备份删除掉

- 生成密钥
 ```shell
 $ ssh-keygen -t rsa -C “user.email”
 ```
 你将会遇到以下情况的处理：
 ```shell
 Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):  直接回车
Enter passphrase (empty for no passphrase):               直接回车
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.  公钥
Your public key has been saved in /root/.ssh/id_rsa.pub.  私钥
The key fingerprint is:
4d:dd:48:af:76:c2:ba:a8:bc:20:f3:28:1d:6a:28:53 
 ```
就是按3次回车，密码为空！
最后你会得到两个文件：`id_rsa`和`id_rsa.pub`
 - 把密钥加到Github或者码云或者Coding的SSH上
 ![Github](/images/github.png)
 我们需要把`id_rsa.pub`中的内容全选复制，然后粘贴进Pages的各自SSH地方，当然需要输入密码。
 - 测试SSH
Github：
```shell
$ssh -T git@github.com
```
码云：
```shell
$ssh -T git@git.oschina.net
```
Coding:
```shell
$ssh -T git@coding.net
```
- 若返回则配置成功
github:
```shell
The authenticity of host ‘github.com (207.97.227.239)’ can’t be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added ‘github.com,207.97.227.239′ (RSA) to the list of known hosts.
```
码云：`Welcome to Git@OSC, yourname!`
Coding：`Enter passphrase for key ‘/c/Users/Yuankai/.ssh/id_rsa’: Coding.net Tips : [ Hello Kyle_lyk! You have connected to Coding.net by SSH successfully! ] `
那么配置好，我们就可以把Hexo部署到Git上了
```shell
$hexo deploy
```

Hexo之Next
==

 - Hexo有很多主题，有大道至简的`maupassant` 也有`casper`还有`uno`。但是我还是最喜欢[next](http://theme-next.iissnan.com/)的风格。


安装Next
======

安装Next是非常的简单。如果你熟悉Git那么你就可以直接使用Git checkout代码：
```shell
$ cd your-hexo-site
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
还有一种方法是下载稳定版本

 - 前往Next版本[发布页面](https://github.com/iissnan/hexo-theme-next/releases)
 - 一直下拉找到`Source code(zip)`点击即可下载
 - 之后把下载的压缩包至站点的themes目录下，并将解压后的文件名改为`next`。

启用Next
--

 - 你需要打开你的站点目录找到`_config.yml`这个文件，记住不是themes下next中的`_config.yml`。
 - 我们需要在站点文件`_config.yml`中`ctrl+F`打入`theme`字段，并将其值改为`next`：
```shell
theme:next
```

验证主题是否生成完成
--
首先我们要右击你的站点然后选择`Git Bash Here`，并开启调试模式。
```shell
$ hexo s --debug
```
命令行出现：
```shell
INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
```
那么我们就可以在网址上输入`http://0.0.0.0:4000`去查看效果，检查站点是否正确运行。

Next各种细节
==

主题设定
--
Scheme 是Next提供的一种特性，就因为Scheme我们有三种选择外观。

 - Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
 - Mist - Muse 的紧凑版本，整洁有序的单栏外观
 - Pisces - 双栏 Scheme，小家碧玉似的清新
Scheme 的切换通过更改 主题配置文件（theme/next下的`_config.yml`），搜索 scheme 关键字。 你会看到有三行 scheme 的配置，将你需用启用的 scheme 前面注释 # 去除即可。
```shell
#scheme: Muse
#scheme: Mist
scheme: Pisces
```
还有更多的小东西大家可以去[Next官方文档](https://github.com/iissnan/hexo-theme-next/releases)看。比如说

 - 菜单，
 - 侧栏，
 - 头像，
 - 作者昵称，
 - 站点描述，
 - 第三方服务等。

Next中foot更改
-----------
![foot](/images/foot.jpg)我把原来的什么Hexo啊..Next.Pisces啊通通改了，这才像我们自己的博客呀！没得说。
我们需要打开`next`下的`layout`接着打开`_partials`下的`footer.swig`。
然后我们要把其中红框里的删除。
![foot1](/images/foot1.png)
紧接着我们回到`next`下，找到`languages`，打开`zh-Hans.yml`。
![foot2](/images/foot2.png)
我们可以改成如下：
```shell
footer:
  powered: "个人专属 "
  theme: Adherent
```
看大家的想法自己随意发挥！
好啦，我要去研究SEO了！！
祝大家建博成功

