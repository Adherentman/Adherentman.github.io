---
title: Mongodb之旅(一)
date: 2017-07-27 21：19
comments: true
layout: post
tags: [Mongodb,数据库]
categories: 数据库,Mongodb
---

# Mongodb之旅(一)

## 插入,insertOne/Many

### insertOne( )

```
db.inventory.insertOne(
   { item: "canvas", qty: 100, tags: ["cotton"], size: { h: 28, w: 35.5, uom: "cm" } }
)
```

<!--more-->

### insertMany([ ])

```
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], size: { h: 14, w: 21, uom: "cm" } },
   { item: "mat", qty: 85, tags: ["gray"], size: { h: 27.9, w: 35.5, uom: "cm" } },
   { item: "mousepad", qty: 25, tags: ["gel", "blue"], size: { h: 19, w: 22.85, uom: "cm" } }
])
```



## 查找,find

### find( )

```
db.inventory.find( {} )
```



## 更新,updateOne/Many

### $set

```mongodb
db.memberplan.update(

{_id:"xxx"},

{$set:

{tags:["coats","outerwear"]}

})
```

###  $currentDate,当前时间

```
db.inventory.updateOne(
   { item: "paper" },
   {
	 $set: { "size.uom": "cm", status: "P" },
	 $currentDate: { lastModified: true }
   }
)

```

### updateMany

```
db.inventory.updateMany(
   { "qty": { $lt: 50 } },
   {
     $set: { "size.uom": "in", status: "P" },
     $currentDate: { lastModified: true }
   }
)
```



