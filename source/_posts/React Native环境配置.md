---
title: React Native iOS/android环境配置
date: 2017-08-14 14：54
comments: true
layout: post
tags: [React,React Native,iOS,Android]
categories: React,React Native,iOS,Android
---

# React Native环境配置

# Ios开发环境

因为我是在mac下搭建环境的。所以比较方便。

- Xcode时必须的！

- > brew install node        //电脑需要有node
  >
  > brew install watchman    //这是用来监视文件系统中的更改的工具

- npm install -g react-native-cli

  然后我们打开 **Xcode**

  ![xcode](/images/xcode.png)

  <!--more-->

接下来我们需要执行命令

**react-native init AwesomeProject**

Then：我们需要在Xcode里打开

![xcode文件](/images/xcode文件.png)

```
接着在终端里
cd AwesomeProject
then：
react-native run-ios
```

接下来我们等就好了

![iphone6](/images/iphone6.png)

这就是成功界面

恭喜🎉！

# Android环境

第一步与ios一样

- brew install node      
- brew install watchman   

我们就不做了。

接下来

## 我们需要安装`Java`的环境。

[Download and install JDK 8 or newer](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

## 安装`Android`环境

1. 安装 **[Android studio](https://developer.android.com/studio/index.html)** 
2. ![custon](/images/custon.png)
3. 勾选`Performance`和`Android Virtual Device`![sdk](/images/sdk.png)
4. 安装完成后，在Android Studio的启动欢迎界面中选择`Configure | SDK Manager`。![config](/images/config.png)
5. 在`SDK Platforms`窗口中，选择`Show Package Details`

![palt](/images/palt.png)

然后tools里

![sdttool](/images/sdttool.png)

![sdktools](/images/sdktools.png)

![sdksuppt](/images/sdksuppt.png)

#### ANDROID_HOME环境变量

确保`ANDROID_HOME`环境变量正确地指向了你安装的Android SDK的路径。具体的做法是把下面的命令加入到`~/.bash_profile`文件中：(**译注**：~表示用户目录，即`/Users/你的用户名/`，而小数点开头的文件在Finder中是隐藏的，并且这个文件有可能并不存在。请在终端下使用`vi ~/.bash_profile`命令创建或编辑。如不熟悉vi操作，请点击[这里](http://www.eepw.com.cn/article/48018.htm)学习）

```
# 如果你不是通过Android Studio安装的sdk，则其路径可能不同，请自行确定清楚。
export ANDROID_HOME=~/Library/Android/sdk

```

然后使用下列命令使其立即生效（否则重启后才生效）：

```
source ~/.bash_profile

```

可以使用`echo $ANDROID_HOME`检查此变量是否已正确设置。

## 最后

同样我们需要在`Android studio`中打开创建的文件夹下Android文件

然后去build

![studio](/images/studio.png)

圆圈是选手机机型。

我们在正方形框框中先点击锤子然后点击绿色箭头之后在终端里输入以下：

```
react-native init AwesomeProject
cd AwesomeProject
react-native run-android
```

下面就是成功画面！恭喜🎉！![Android](/images/Android.png)

# 在搭建环境下踩得坑

## Android下

![安卓bug](/images/安卓bug.png)

这是我运行安卓的时候遇到的问题。我们需要在 **Android studio**先启动一个手机模拟器再去终端里输入指令：`react-native run-android` 就可以了





![android tools](/images/android tools.png)

这个问题是说Build Tools 23.0.0.1太低至少需要25.0.0

但是我升级过后还不行。发现是配置文件里的问题。

![gaibuild](/images/gaibuild.png)

这样就行啦！



## Ios下

具体出错问题描述找不到了，但是情况还是记得的。

就是说我ios的虚拟机打开了但是我在上面看不见自己的项目。

这时候google到答案。。

![ios解决问题1](/images/ios解决问题1.png)

需要开一个终端，然后去在你的项目下`npm install` 就是这样。。具体情况我也不知道发生了什么哈哈哈