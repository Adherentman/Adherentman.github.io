---
title: ReactNative热更新之微软CodePush
date: 2018-03-28 22：09
comments: true
layout: post
tags: [React Native]
categories: React Native
---

# 微软 CodePush 使用

## 安装

`yarn global add code-push-cli`

## 注册账户

`code-push register`

会弹出一个浏览器,让你注册,可以使用 github 帐号对其进行授权,授权成功会给一串 Token,点击复制，在控制进行粘贴回车(或者使用 code-push login 命令)。

<!--more-->

## CodePush 登录相关命令

* code-push login 登陆
* code-push logout 注销
* code-push access-key ls 列出登陆的 token
* code-push access-key rm 删除某个 access-key

## CodePush 创建 App 相关命令

* code-push app add
  * 如: code-push app add yimutest-ios ios/android react-native
* code-push app add
  * 在账号里面添加一个新的 app
* code-push app remove
  * rm 在账号里移除一个 app
* code-push app rename
  * 重命名一个存在 app
* code-push app list
  * ls 列出账号下面的所有 app
* code-push app transfer
  * 把 app 的所有权转移到另外一个账号

## 在项目中  加包

1.  `yarn add react-native-code-push`
1.  `react-native link`

之后会让你将刚才添加的 ios/Android App 的 Staging Key 复制粘贴到这里

Staging 为测试的 key，Production 为生产打包时用的 key。

如果忘了  则在终端下做查看:

`code-push deployment ls 您的应用名 -k`

## App.js

```javascript
import codePush from "react-native-code-push";
const codePushOptions = { checkFrequency: codePush.CheckFrequency.MANUAL };

componentDidMount(){
  codePush.sync({
    updateDialog: true,
    installMode: codePush.InstallMode.IMMEDIATE,
    mandatoryInstallMode:codePush.InstallMode.IMMEDIATE,
    //deploymentKey为刚才生成的,打包哪个平台的App就使用哪个Key,这里用IOS的打包测试
    deploymentKey: 'xxxxxxx',
    });
}
```

附上其他的详细 API: [详细配置](https://github.com/Microsoft/react-native-code-push/blob/master/docs/api-js.md)

## 发布新版本

> `code-push release <应用名称> <Bundles所在目录> <对应的应用版本> --deploymentName： 更新环境 --description： 更新描述 --mandatory： 是否强制更新`

> `code-push release codepush01 ./bundles/ 1.0.0 --deploymentName Production --description "我也不知道要写啥" --mandatory false`

1.  `code-push app add aaa-ios ios react-native`
1.  `yarn start-ios`

### Android 新版本发布

修改 App.js 的 deploymentKey 为安卓的

1.  `deploymentKey:'android key'`
1.  `code-push release-react aaa-android android`
1.  `yarn start-android`
