---
title: map
date: 2023-12-04 19:42:12
tags:
- javaScript
categories:
- programming
- JavaScript
---

## Map

### 基本信息
- 定义：一个带键的数据项集合
- 区别：像object一样，它们最大的差别是Map允许任何类型的键 (key)
- 方法和属性：
	- new Map()：创建map
	- map.set(key,value)：根据键存储值
	- map.get(key)：获取键对应的值，如不存在相应的键，返回undefined
	- map.has(key)：是否存在，key存在返回true，否则false
	- map.delete(key)：删除指定键的值
	- map.clear()：清空map
	- map.size()：返回当前元素个数

### Objects&maps
- 相同
	- 允许按键存取一个值
	- 删除键
	- 检测一个键是否绑定了值
- 不同
	- 意外的键
		- map：默认不包含任何键
		- object：默认有一个原型，原型链上的键名可能与设置的对象键名冲突
	- 键的类型
		- map：任意值
		- object：必须是string或者symbol
	- 键的顺序
		- map：有序，因此可以根据插入的顺序返回键值
		- object：无序
	- Size
		- map：可以直接获取长度
		- object：不能直接获取长度，只能通过手动计算
	- 性能
		- map：在频繁增删键值对的场景表现更好
		- object：频繁增删键值对的场景未作优化

### Map的链式调用
> 每次map.set调用都返回map本身，所以可以进行"链式"调用

```js
map.set('1', 'str1')
	.set(1, 'num1');
	
// 等价于

map.set('1', 'str1');
map.set(1, 'num1');
```

### Map的初始化
- 创建Map时可以传入一个带有键值对的数组(或其他可迭代对象)来初始化
- 即使用常规的Map构造函数可以将一个二维键值数组转换为Map对象
```js
let map = new Map([
	['1', 'str1'],
	[1, 'num1']
])
```

### Map迭代

|     方法      |         描述         |
| :-----------: | :------------------: |
|  map.keys()   |  遍历并返回所有的键  |
| map.values()  |  遍历并返回所有的值  |
| map.entries() | 遍历并返回所有的实体 |

### Map和forEach
> Map有内置的foreach，与Array类似

```js
myMap.forEach((value, key, map) => {
	console.log(`${key} : ${value}`)
})
```

### Map和Objects互相转化
- 已有对象obj转map
```js
let obj = {
	name : 'xx',
	age : 12
};

let map = new Map(Object.entries(obj));
```
- 已有map转obj
```js
let prices = Object.fromEntries([
	['banana', 1],
	['orange', 2]
]);

// => {banana:1, orange:2}
```

### 复制合并Maps
- Map复制
```js
let clone = new Map(original);
```

- Map合并
```js
let merged = new Map([...first, ...second])
```
