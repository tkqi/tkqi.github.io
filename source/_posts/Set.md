---
title: Set
date: 2023-11-20 09:26:19
tags:
---

## Set基本用法

- Set是ES6提供的新的数据结构
- 类似于数组
- 成员是唯一的，没有重复的值

### 初始化

```js
// 新建一个Set变量
const s = new Set();

// 初始化,add方法往set中添加元素
[2, 3, 5, 4, 5, 2, 2].forEach(x=>s.add(x));
```

### set中的重复问题

```js
// 新建set时接收一个数组作为参数
const set = new Set([1, 2, 3, 4, 4]); //{1, 2, 3, 4}
var arr = [...set]; // [1, 2, 3, 4] type:object

// set中的NaN等于自身，即重复
let set3 = new Set();
let a = NaN;
let b = NaN;
set3.add(a).add(b);//{NaN}

// set中对象总是不等
let set4 = new Set();
set4.add({}).add({});// {{}, {}}
```

### set常用方法

```js
const set = new Set();
set.add(1).add(2);// { 1, 2 }

// 判断在set中是否包含某个值
var flag = set.has(1);//true

// 删除某个元素
set.delete(1);//set : { 2 }

// 在set中键名=键值
// 键名
var key = set.keys();// { 1, 2 }
// 键值
var val = set.values();// { 1， 2 }
// 键值对
var entries = set.entries();//{ [ 1, 1 ], [ 2, 2 ] }

// 遍历
// for循环
for (let item of set) {/*数据处理*/ };
// forEach方法
set.forEach((item, index) => {/*数据处理*/ })；
```

### 与数组相关操作

```js
// set转为数组
const set = new Set([1, 2, 3, 4]);
const arr = Array.from(set);// [1, 2, 3, 4]

// 数组去重
const newArr = Array.from(new Set(arr));

// 通过 ... 运算转换为逗号分隔的序列
// map方法
// 通过map方法将数组映射为新数组
const newArr1 = Array.from(new Set([...set].map(o => o * 2))); console.log([...set]);

// filter方法
// 通过filter方法返回过滤后的数组
const newArr2 = Array.from(new Set([...set].map(o => o >= 1))); console.log([...set]);

```

