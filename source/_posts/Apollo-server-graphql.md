---
title: Apollo-server-graphql
date: 2018-05-08 21：37
comments: true
layout: post
tags: [GraphQL]
categories: [GraphQL]
---

# Apollo-server-graphql

玩 `graphql` 的肯定听过吧 `apollo` 等等？没有？
上月球的那个 `apollo`也没听过吗？？

## apollo-server主要是做什么呢？？
它能轻松将您的后端转换为 `GraphQL` 架构。
你说你们以前都是 `rest` ？
没事，最新版的 `apollo` 能够让你，
在现有的 `REST API` 之上构建通用的 `GraphQL API`，这样您就可以快速发布新的应用程序功能，而无需等待后端更改。

<!--more-->

## Go！
我用 `TypeScript` 和 `Koa` 去玩 `graphql` 。听起来就很棒！
> yarn add koa koa-router apollo-server-koa graphql-tools koa-bodyparser
> && yarn add  @types/graphql @types/koa @types/koa-bodyparser @types/koa-router  typescript -D
```typescript
import * as Koa from 'koa';
import * as Mongoose from 'mongoose';
import * as koaRouter from 'koa-router';
import * as koaBody from 'koa-bodyparser';
import { graphiqlKoa,graphqlKoa } from 'apollo-server-koa';
import { makeExecutableSchema } from 'graphql-tools';
import env from './env';

const app = new Koa();
const router = new koaRouter();
const port: number = 4040;

app.use(koaBody());

// 注册路由中间件
app.use(router.routes()).use(router.allowedMethods());

const books = [
  {title: "hdiahida", author: 'dhiahdia'},
  {title: "dbfb", author: 'dahdiahi'}
];

const typeDefs = `
  type Query { books: [Book]}
  type Book { title: String, author: String}
`;

const resolvers = {
  Query: { books: () => books}
};

const schema = makeExecutableSchema({
  typeDefs,
  resolvers
});

router.post(
  '/graphql',
  graphqlKoa({
    schema,
  })
);

// graphiql
router.get(
  '/graphiql',
  graphiqlKoa({
    endpointURL: '/graphql',
  })
);

router.get('/404', async ctx => {
  ctx.body = '404!!!';
});

app.listen(port, ()=>{
  console.log('🌏 => server is open loaclhost:' + port)
});
```