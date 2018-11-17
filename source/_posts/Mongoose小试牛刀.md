---
title: Mongoose小试牛刀
date: 2018-05-07 19：33
updated: 2018-05-08 21:49:29
comments: true
layout: post
tags: [Mongodb,数据库]
categories: [数据库]
---
# Mongoose小试牛刀

## 认识下Mongodb：
* 文档(document) 
  ![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/mongodb-Document.png)
* 集合（Collection）
  * document存于collection
* 数据库（database）
  * 里面有很多collection

<!--more-->
## 在mongoose中：
* Schema -> collection，不具备数据库的操作能力
* Model -> 由Schema生成的模型，能操作数据库
* Entity -> 由Model创建的实例，也能操作数据库

## 创建schema
```typescript
import * as mongoose from 'mongoose';

export interface UserType extends mongoose.Document {
  name: String
}
export let userSchema = new mongoose.Schema({
  name: String,
}, {collection: 'user'})

let User = mongoose.model<UserType>('User', userSchema)

export default User;
```
在这里我们会遇到一个问题，如果我们不添加`collections: 'user'`之后会发现会创建一个名为 `users` 的 `collection` 的表名。
还有个方法自定义 `collection` 名：
```typescript
let User = mongoose.model<UserType>('User', userSchema, 'user')
```
二选一！
## 创建model
1. 
```typescript
import User, { UserType } from '../schemas/User';

let UserModel = new User({name: "xzh"});
UserModel.save(((err: any, product: UserType )=> {
  if(err) return console.error(err);
  console.log(product, '结果');
}))

export default UserModel;
```
这有个小插曲，我们也可以不定义一个 `UserModel` 去向数据库写入数据：
2. 
```typescript
import User, { UserType } from '../schemas/User';

export default User.create({name: "xzh"}, (err: any, res: any) => {
  if(err) return console.error(err);
  console.log(res, '结果')
})
```

因为前面也提到了在 `mongoose` 中还有个 `Entity` 的存在。
第1种是 `entity` 保存方法。
第2种是 `model` 保存方法。