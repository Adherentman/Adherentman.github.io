---
title: 打造个人or团队适用的开源项目规范
date: 2018-10-03 19：33
comments: true
layout: post
tags: [工程化]
categories: [工程化]
---

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1541259226466&di=3d9d04f5f77d2f23b7b121fb02083038&imgtype=jpg&src=http%3A%2F%2Fimg4.imgtn.bdimg.com%2Fit%2Fu%3D738639994%2C2302403857%26fm%3D214%26gp%3D0.jpg)

## lerna
Lerna 是一个用来优化托管在git\npm上的多package代码库的工作流的一个管理工具,可以让你在主项目下管理多个子项目，从而解决了多个包互相依赖，且发布时需要手动维护多个包的问题。

lerna的文件树：
```
my-lerna-repo/
  package.json
  packages/
    package-1/
      package.json
    package-2/
      package.json
```

首先作为项目拥有者全局安装：
```shell
$ npm install -g lerna
# or
$ yarn global add lerna
```

### init
在项目使用以下命令：

<!--more-->

```shell
$ lerna init
# or
$ lerna init --independent
```

这条命令要注意的是，lerna提供两类管理项目的模式：
1. fixed/locked mode（default）
	1. Fixed模式下，项目通过单一的版本进行控制。版本号放在项目根目录下的lerna.json文件的version这个字段。当你执行 lerna publish，如果有文件更新，它将发布新的版本。
2. independent mode（—independent）
	1. 这种模式下，项目里的各个package独立维护自己的version，它将会忽略lerna.json中定义的version

### publish
```shell
$ lerna publish
```
Publish它做了以下几件事情
1. 发布项目里的每个模块
2. 执行lerna updated确定是否需要发布
3. 假如需要发布 给lerna.json 版本号做自增
4. 更新package.json里的版本号至最新
5. 为新版本更新dependencies
6. 为新版本创建一个git commit 和tag
7. 发布更新项目到npm
8. 一次发布所有packages
只有你在`package.json`里设置`private: true`这个包则不会被发布。

### 配置
如果我们使用yarn我们可以在`lerna.json`做如下配置：
```json
{
...
  "npmClient": "yarn"
...
}
```


## 代码检查和规范
在一个项目中，多人开发时会遇到代码格式问题。
解决方案：
### 使用Eslint
项目拥有者需要全局安装：
```shell
$ npm install eslint -g
# or
$ yarn global add eslint
```

在项目中执行命令：
```shell
$ eslint --init
```

React项目推荐使用airbnb规范。
```json
{
    "devDependencies": {
        "eslint": "^4.19.1",
        "eslint-config-airbnb": "^17.0.0",
        "eslint-plugin-import": "^2.13.0",
        "eslint-plugin-react": "^7.10.0",
        "eslint-plugin-jsx-a11y": "^6.1.1"
    }
}
```

### 使用Standard
参考中文文档 [standard/README-zhcn.md at master · standard/standard · GitHub](https://github.com/standard/standard/blob/master/docs/README-zhcn.md#i-disagree-with-rule-x-can-you-change-it)
### Prettier
Eslint + prettier 解决了代码风格和格式化的所有问题。

### 与git hook来解决何时lint

目前比较成熟的是`husky`与`lint-staged`两者结合:
```shell
$ npm install lint-staged husky -D
# or
$ yarn add lint-staged husky -D
```

接着在`package.json`中加入：
```json
"lint-staged": {
    "packages/*/src/**/*.js": [
      "eslint --fix",
      "prettier --write",
      "git add"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
```

### Tslint
如果你的项目使`TypeScript`那么`tslint`能帮助你许多！

```shell
$ yarn add -D prettier tslint-config-prettier tslint-plugin-prettier husky pretty-quick
```

#### Tslint.json
```json
{
    "defaultSeverity": "error",
    "extends": [
				"tslint:recommended",
				"tslint-react",
				"tslint-config-prettier"
    ],
    "jsRules": {},
    "rules": {
			"prettier": [true, "./prettierrc"]
		},
    "rulesDirectory": [
			"tslint-plugin-prettier"
		]
}
```

## 编辑器的不同
解决方案：
主流的都是使用EditorConfig
只需要在根目录新建一个`.editorconfig`文件，然后去根据文档自行定义。再给自己使用的编辑器安装`editorConfig`的插件即可。
官方网站：[EditorConfig](https://editorconfig.org/)

## commit 不统一
主流的方法是commitizen.
项目拥有者应当全局安装：
```shell
$ npm install -g commitizen
# or
$ yarn global add commitizen
```

之后我们在项目里选用angular格式的commit message并在终端下输入以下命令：
```shell
$ commitizen init cz-conventional-changelog —save-exact
# or
$ commitizen init cz-conventional-changelog --yarn --dev --exact
```
上面的命令为你做了三件事：
1. 安装cz-conventional-changelog的adapter的npm模块。
2. 将其保存到package.json的dependencies或devDependencies。
3. 将config.commitizen键添加到package.json的根目录，如下所示：
```json
"config": {
    "commitizen": {
      "path": "cz-conventional-changelog"
    }
  }
```
然后在`package.json`里的`scripts`里加上
```json
"scripts": {
    "commit": "git-cz"
 }
```
注意：
如果你使用像`husky`这样的`precommit hooks`，你需要为脚本命名除“commit”之外的其他东西（例如“cm”：“git-cz”）。
原因是因为npm-scripts有一个“feature”，它自动运行名称为prexxx的脚本，其中xxx是另一个脚本的名称。
本质上，如果您将脚本命名为“commit”，则npm和husky将运行两次“precommit”脚本，并且解决方法是阻止npm触发的precommit脚本。

之后如果有别人也参与进你的项目开发中，我们最好在仓库里也安装依赖
```shell
$ npm install -D commitizen
# or
$ yarn add commititzen -D
```

## changelog
通过手动去维护changelog在我之前的那个wx-tsApi项目中是非常头疼的一件事情，所以去寻找自动化的东西。

全局安装
```shell
$ npm install -g conventional-changelog-cli
# or
$ yarn global add conventional-changelog-cli
```
然后在`package.json`文件中添加`scripts`
```json
{
	"scripts": {
    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -s -r 0 && git add CHANGELOG.md"
	}
}
```

官方推荐的工作流程：
1. 做出改变
2. 提交这些改变
3. 确定Travis变成绿色
4. 改version
5. changelog
6. commit package.json 和CHANGELOG.md文件
7. 打Tag
8. push

基于lerna的工作流程(自己研究的有不对请指出)：
1. 做出改变
2. git-cz
3. "conventional-changelog -p angular -i CHANGELOG.md -s -r 0 && git add CHANGELOG.md"
4. commit CHANGELOG.md 文件
5. Git push
6. lerna publish

## 借鉴
- eJayYoung([如何打造规范的开源项目workflow · Issue #1 · eJayYoung/blog · GitHub](https://github.com/eJayYoung/blog/issues/1))