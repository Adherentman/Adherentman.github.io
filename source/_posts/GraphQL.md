---
title: GraphQL基本操作
date: 2017-07-22 21：16
comments: true
layout: post
tags: [GraphQL]
categories: GraphQL
---

# GraphQL()

强类型

也就是说，你可以查询值类型：`Int`, `Float`, `String`, `Boolean`和`ID`

GraphQL不是像MySQL或Redis这样直接面向数据的接口，而是面向你已经存在的应用代码的接口。

你可以把GraphQL看作是为了调用应用服务器上的方法的一些内嵌的RPC。

## 操作(operation)

**操作（Operations）**

GraphQL 规范支持两种操作：

- query：仅获取数据（fetch）的只读请求
- mutation：获取数据后还有写操作的请求

```json
query{
    clent(id:1){
      id
      name
  }
}
```
- client 是查询的operation
- (id:1)包含了传入给Query的参数
- 查询包含id和name字段,这些字段也是我们希望查询可以返回的.

<!--more-->

server会给这个查询返回什么：

```json
{
  "data": {
    "client": {
      "id": "1",
      "name": "Uncle Charlie"
    }
  }
}
```

server会返回一个JSON串。这个JSON的schema和查询的基本一致。

## 变量(Variable)

```json
query($clientId: Int) {
  client(id: $clientId) {
    name
    dob
  }

  purchases(client_id: $clientId) {
    date
    quantity
    total
    product {
      name
      price
      product_category {
        name
      }
    }
    client {
      name
      dob
    }
  }
}
```

```json
{
  "clientId": 1
}
```

# Graphql的schema下

```
schema{	

	query: Query,

	mutation: Mutation,

}
```

## Mutation（修改）

增、删、改一类的operation在GraphQL里统称为**变异（mutation，即修改数据）**

GraphQL中将对数据的修改操作称为 mutation。在 GraphQL Schema 中按照如下形式来定义一个 mutation：

mutation 查询和普通查询请求（query）的重要区别在于 mutation 操作是序列化执行的。例如 GraphQL 规范中给出的示例，服务器一定会序列化处理下面的 mutation 请求：

请求结束时 theNumber 的值会是 2。

- create_client增加

```json
mutation {
  create_client (
    name: "查理大叔"
    dob: "2017/01/28"
  ) {
    id 
    name
    dob
  }
}
```

- update_client更新

```json
mutation {
  update_client (
    id: 5
    dob: "1990/01/01"
  ) {
    id
    name
    dob
  }
}
```

- destroy_client删除

```json
mutation {
  destroy_client(id: 5) {
    name 
    dob
  }
}
```

# 修改数据

就像Rest以PUT／POST约定为修改服务器端数据一样，Mutations操作在GraphQL的意义就是修改数据库。就像官网中的例子：

```
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) { //!表示必须填写的查询条件  
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}
{
  "ep": "JEDI",
  "review": {
    "stars": 5,
    "commentary": "This is a great movie!"
  }
}
```

需要注意的是，为了保证mutation操作不冲突，mutation只能序列执行。而query可以并行。

## 强类型

由于 GraphQL 是一个强类型语言，所以它可以在执行查询之前检查每个查询语句是否满足事先设定的 schema，符合则合法，如果查询语句不合法则不进行查询。

## Fragments(组合)

GraphQL 可以组合使用查询。比如可以定义一种叫 fragment 的东西，就是查询片断，然后我们可以在不同的地方重复的去使用查询。比如下面的这个例子：

```json
{
  me {
    name
    friends {
      name
      events {
        name
      }
    }
  }
}
```

可以转换成这样：

```json
{
  me {
    name
    friends {
      ...firendFragment
    }
  }
}

fragment friendFragment on User {
  name
  events {
    name
  }
}
```

上面定义了一个叫 friendFragment 的查询片断，它返回用户朋友的名字，还有参加的活动的名字，然后我们可以在其它的查询里面使用这个查询片断。



简单的说，GraphQL 是一种**描述请求数据方法的语法**，通常用于客户端从服务端加载数据。GraphQL 有以下三个主要特征：

- 它允许客户端指定具体所需的数据。
- 它让从多个数据源汇总取数据变得更简单。
- 它使用了类型系统来描述数据。

一个 GraphQL API 主要由三个部分组成：**schema（类型）**，**queries（查询）** 以及 **resolvers（解析器）**。



# Arguments(参数)

```JavaScript
{
  human(id: "1000") {
    name
    height
  }
}
```

```javascript
{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 1.72
    }
  }
}
```

当然在字段里我们也可以传参数.

```JavaScript
{
  human(id: "1000") {
    name
    height(unit: FOOT//or METER)
  }
}
```

```JavaScript
{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 5.6430448 // or 1.72
    }
  }
}
```

# Aliases(别名)

```json
{
  empireHero: hero(episode: EMPIRE) {
    name
  }
  jediHero: hero(episode: JEDI) {
    name
  }
}
```

```json
{
  "data": {
    "empireHero": {
      "name": "Luke Skywalker"
    },
    "jediHero": {
      "name": "R2-D2"
    }
  }
}
```

## Variables(变量)

当我们开始使用变量的时候,我们需要做三件事情

> 1. Replace the static value in the query with `$variableName`
> 2. Declare `$variableName` as one of the variables accepted by the query
> 3. Pass `variableName: value` in the separate, transport-specific (usually JSON) variables dictionary

1. 用`$` 替换查询中的静态值
2. 将`$` 声明为查询接受变量之一
3. 通常传递json.

```javascript
query
HeroNameAndFriends(
	$episode: Episode,
	){
	hero(
	episode: $episode
){
	name
 	friends{
  	name
 	}
  }
}

//variables
{
  "episode": "JEDI"
}
```

```javascript
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```



# fields

Fields就是客户端要求GraphQL返回的数据说明，一个Fields也可以包含参数

# Root fields & resolvers

```
Query: {
  human(obj, args, context) {
    return context.db.loadHumanByID(args.id).then(
      userData => new Human(userData)
    )
  }
}
```

obj上一个对象，其对于根查询类型的字段通常不被使用

args提供给GraphQL查询中的字段的参数。

context提供给每个解析器并保存重要的上下文信息（如当前登录的用户）或访问数据库的值。



