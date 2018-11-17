---
title: JavaScript基本概念（高程3）
date: 2017-04-11 10:49
comments: true
layout: post
tags: [JavaScript]
categories: Javascript修仙之旅
---

# JavaScript基本概念（高程3）



## 数据类型

### typeof操作符

"undefined"——如果这个值未定义;

"boolean"——如果这个值是布尔值;

"string"——如果这个值是字符串;

"number"——如果这个值是数值;

"object"——如果这个值是对象或null;

"function"——如果这个值是函数;
<!--more-->
### Undefined类型

- Undefined类型只有一个值，即特殊的undefined。

### Null类型

- Null类型是第二个只有一个值的数据类型，这个特殊的值是null。从逻辑角度来看，null值表示一个空对象指针，而这也正是使用typeof操作符检测null值会返回"object"的原因。

### Boolean类型

- Boolean类型是ECMAScript中使用得最多的一种类型，该类型只有两个字面值：true和false。这两个值与数字值不是一回事，**因此true不一定等于1，而false也不一定等于0.**

### Number类型

整数通过八进制（以8为基数）或十六进制（以16为基数）的字面值来表示。其中，八进制字面值的第一位必须是零（0），然后是八进制数字序列（0~7）。

十六进制字面值的前两位必须是0x，后跟任何十六进制数字（0~9及A~F）。

- 浮点数值

永远不要测试某个特定的浮点数值。

如果小数点后面没有跟任何数字，那么这个数值就可以作为整数值来保存。

- 数值范围

```javascript
var result= Number.MAX_VALUE +Number.MAX_VALUE;
alert(isFinite(result));			//false
```



- NaN

NaN，即非数值（Not a Number）是一个特殊的数值，这个数值用于表示一个本来要返回数值的操作数未返回数值的情况。

它本身有两个非同寻常的特点。首先，任何涉及NaN的操作（例如NaN/10）都会返回NaN，这个特点在多步计算中有可能导致问题。其次，NaN与任何值都不想等，包括NaN本身。例如，

```javascript
alert(NaN == NaN);  		 //false
```

- 数值转换

  ```javascript
  var num1 = Number("Hello world");		//NaN
  var num2 = Number(" ");					//0
  var num3 = Number("000011");			//11
  var num4 = Number("true");				//1
  ```

  ### String类型

- 字符字面量-也叫转义序列

- 字符串的特定

字符串一旦创建，它们的值就不能改变。

- 转换为字符串

要把一个值转换为一个字符串有两种方式。第一种是使用几乎每个值都有的toString()方法。第二用String方法。

### Object类型

- constructor:保存着用于创建当前对象的函数。

- hasOwnProperty（propertyName）:用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中作为参数的属性名（propertyName）必须以字符串形式指定（例如：o.hasOwnProperty("name")）。

- isPrototypeOf(object):用于检查传入的对象是否是传入对象的原型。

- propertyIsEnumerable(propertyName)：用于检查给定的属性是否能够使用for-in语句来枚举。与2方法一样，作为参数必须以字符串形式指定。

- toLocaleString():返回对象的字符串表示，该字符串与执行环境的地区对应。

- toString():返回对象的字符串表示。

- valueOf():返回对象的字符串、数值或布尔值表示。通常与toString()方法的返回值相同。

  ## 操作符

  ## 一元操作符

  - 递增和递减操作符

  ​     递增和递减操作符直接借鉴C。

  - 位操作符

  符号位的值决定了其他为数值的格式。

  **负数同样以二进制码存储，但使用的格式是二进制补码**



- 按位非（NOT）

按位非操作符由一个波浪线（~）表示，执行按位非的结果就是返回数值的反码。

```javascript
var num1 = 25;//二进制00000000000000000000000000011001
//二进制11111111111111111111111111100110
var num2 = ~num1;
alert(num2);//-26
```

- 按位与（AND）

| 第一个数值的位 | 第二个数值的位 |  结果  |
| :-----: | :-----: | :--: |
|    1    |    1    |  1   |
|    1    |    0    |  0   |
|    0    |    1    |  0   |
|    0    |    0    |  0   |

简而言之，按位与操作只在两个数值的对应位都是1时才返回1，任何一位是0，结果都是0.

- 按位或（OR）

| 第一个数值的位 | 第二个数值的位 |  结果  |
| :-----: | :-----: | :--: |
|    1    |    1    |  1   |
|    1    |    0    |  1   |
|    0    |    1    |  1   |
|    0    |    0    |  0   |

按位或操作在有一个位是1的情况下就返回1，而只有在两个位都是0的情况下才返回0.

- 按位异或（XOR）

| 第一个数值的位 | 第二个数值的位 |  结果  |
| :-----: | :-----: | :--: |
|    1    |    1    |  0   |
|    1    |    0    |  1   |
|    0    |    1    |  1   |
|    0    |    0    |  0   |

按位异或与按位或的不同之处在于，这个操作两个数值对应位上只有一个1时才返回1，如果对应的两位都是1或是0，则返回0.

- 左移

左移操作符由两个小于号（<<）表示。

```javascript
var oldValue = 2;					//等于二进制的10
var newValue = oldValue << 5;//等于二进制的1000000,十进制的64
```



- 有符号的右移

有符号的右移操作符由两个大于号（>>）表示。

```javascript
var oldValue = 64;//等于二进制的1000000
var newValue = oldValue >> 5;//等于二进制的10，即十进制的2
```

- 无符号右移

无符号右移操作符由3个大于号（>>>）表示。

```javascript
var oldValue = 64;//等于二进制的1000000
var newValue =oldValue >>> 5;//等于二进制的10，即十进制的2
```

无符号右移操作符会把负数的二进制码当成正数的二进制码。而且由于负数以其绝对值的二进制补码形式表示，因此就会导致无符号右移后的结果非常之大。

```javascript
var oldValue = -64;//等于二进制的11111111111111111111111111000000
var newValue = oldValue >>>5;//等于十进制的134217726
```

## 布尔操作符

- 逻辑非（！）

```javascript
alert(!false);	//true
alert(!"blue");	//false
alert(!0);		//true
alert(!NaN);	//true
alert(!"");		//true
alert(!12345);	//false
```

- 逻辑与（&&）

| 第一个操作数 | 第二个操作数 |  结果   |
| :----: | :----: | :---: |
|  true  |  true  | true  |
|  true  | false  | false |
| false  |  true  | false |
| false  | false  | false |

在有一个操作数不是布尔值的情况下，逻辑与操作就不一定返回布尔值。

- 逻辑或

|  true  |  true  | true  |
| :----: | :----: | :---: |
|  true  | false  | true  |
| false  |  true  | true  |
| false  | false  | false |
| 第一个操作数 | 第二个操作数 |  结果   |

与逻辑与操作相似，如果有一个操作数不是布尔值，逻辑或也不一定返回布尔值，
