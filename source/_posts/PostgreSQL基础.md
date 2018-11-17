---
title: PostgreSQL基础
date: 2018-08-26 22:09
updated: 2018-09-16 15:18:20
comments: true
layout: post
tags: [数据库, PostgreSQL]
categories: [数据库, PostgreSQL]
---

# PostgreSQL基础

创建新表：

```sql
CREATE TABLE xxx (
	xxx		int,	# 普通的整数类型
	xxx		varchar(80), # 一个可以存储最长 80 个字符的任意字符串的数据类型
	xxx		real,		# 一种用于存储单精度浮点数的类型
	xxx		date		# 时间
)
```

删表：
```sql
DROP TABLE tablename;
```

在表中增加行：
```sql
INSERT INTO tablename (xx, xx, xx)
		VALUES(xx, xx, xx)
```
<!--more-->
## 查询一个表:
```sql
# * 代表所有列的缩写
SELECT * FROM tablename
# 或者单个查
SELECT xxx, xxx, xxx FROM tablename
# 带函数的
SELECT xx, (mobile+number)/2 AS temp_avg, date FROM xx;
# 指定需要哪些行
# WHERE子句包含一个布尔（真值）表达式，只有那些使布尔表达式为真的行才会被返回 在条件中可以使用常用的布尔操作符（AND、OR和NOT）
SELECT * FROM weather
    WHERE city = 'San Francisco' AND prcp > 0.0;
# 查询结果排好序
SELECT * FROM weather
    ORDER BY city;
# 消除重复行
SELECT DISTINCT city
    FROM weather;
# 组合使用DISTINCT和ORDER BY来保证获取一致的结果
SELECT DISTINCT city
    FROM weather
    ORDER BY city;
```

## 连表查询：
假如我们有一个表t1：
```
 num | name
-----+------
   1 | a
   2 | b
   3 | c
```
t2：
```
 num | value
-----+-------
   1 | xxx
   3 | yyy
   5 | zzz
```

### 如果两个表分别有 N 和 M 行，连接表将有 N * M 行。

`FROM T1 CROSS JOIN T2`等效于`FROM T1 INNER JOIN T2 ON TRUE`（见下文）。它也等效于`FROM T1,T2`
```sql
SELECT * FROM t1 CROSS JOIN t2;
 num | name | num | value
-----+------+-----+-------
   1 | a    |   1 | xxx
   1 | a    |   3 | yyy
   1 | a    |   5 | zzz
   2 | b    |   1 | xxx
   2 | b    |   3 | yyy
   2 | b    |   5 | zzz
   3 | c    |   1 | xxx
   3 | c    |   3 | yyy
   3 | c    |   5 | zzz
(9 rows)
```

T1表的 num 和T2表的num 相等的数据：
```sql
SELECT * FROM t1 INNER JOIN t2 ON t1.num = t2.num;
 num | name | num | value
-----+------+-----+-------
   1 | a    |   1 | xxx
   3 | c    |   3 | yyy
(2 rows)
```

是上面的简写，并且`JOIN USING`的输出会**废除冗余列**:
```sql
SELECT * FROM t1 INNER JOIN t2 USING (num);
 num | name | value
-----+------+-------
   1 | a    | xxx
   3 | c    | yyy
(2 rows)
```

`NATURAL`是`USING`的缩写形式:
该列表由那些在两个表里都出现了的列名组成
```sql
SELECT * FROM t1 NATURAL INNER JOIN t2;
 num | name | value
-----+------+-------
   1 | a    | xxx
   3 | c    | yyy
(2 rows)
```

```sql
SELECT * FROM t1 LEFT JOIN t2 ON t1.num = t2.num;
 num | name | num | value
-----+------+-----+-------
   1 | a    |   1 | xxx
   2 | b    |     |
   3 | c    |   3 | yyy
(3 rows)
```

生成的连接表里为来自 T1 的每一行都至少包含一行。
```sql
SELECT * FROM t1 LEFT JOIN t2 USING (num);
 num | name | value
-----+------+-------
   1 | a    | xxx
   2 | b    |
   3 | c    | yyy
(3 rows)
```

因此，生成的连接表里为来自 T2 的每一行都至少包含一行。
```sql
SELECT * FROM t1 RIGHT JOIN t2 ON t1.num = t2.num;
 num | name | num | value
-----+------+-----+-------
   1 | a    |   1 | xxx
   3 | c    |   3 | yyy
     |      |   5 | zzz
(3 rows)
```

为 T2 中每一个无法在连接条件上匹配 T1 里任何一行的行返回一个连接行，该连接行中 T1 的列用空值补齐。
```sql
SELECT * FROM t1 FULL JOIN t2 ON t1.num = t2.num;
 num | name | num | value
-----+------+-----+-------
   1 | a    |   1 | xxx
   2 | b    |     |
   3 | c    |   3 | yyy
     |      |   5 | zzz
(4 rows)
```

```sql
SELECT * FROM t1 LEFT JOIN t2 ON t1.num = t2.num AND t2.value = 'xxx';
 num | name | num | value
-----+------+-----+-------
   1 | a    |   1 | xxx
   2 | b    |     |
   3 | c    |     |
(3 rows)
```

```sql
SELECT * FROM t1 LEFT JOIN t2 ON t1.num = t2.num WHERE t2.value = 'xxx';
 num | name | num | value
-----+------+-----+-------
   1 | a    |   1 | xxx
(1 row)
```
## 更新数据
```sql
UPDATE products SET xx = 10 WHERE xx = 5;
```