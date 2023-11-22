---
title: filter
date: 2023-11-22 10:08:57
tags: Vue
categories: programming
---

### 过滤器
- vue.js允许自定义过滤器，可被用于常见的文本格式化
- 过滤器可以用到的地方
	- 双花括号插值
	- v-bind表达式
- 其中过滤器被添加在JavaScript表达式的尾部，由管道符号指示--" | "

```vue
<!--在双花括号中-->
{{ message | capitalize }}

<!--在v-bind中-->
<div v-bind:id="rawId | formatId"></div>
```

#### 本地过滤器
```js
filters:{
	capitalize:function(value){
		if(!value)return''
		value = value.toString()
		return value.chatAt(0).toUpperCase()+value.slice(1)
	}
}
```

#### 全局过滤器
```js
Vue.filter('capitalize',function(value){
	if(!value)return''
	value = value.toString()
	return value.chatAt(0).toUpperCase()+value.slice(1)
})

new Vue({
...
})
```

#### 过滤器串联
```vue
<!--先通过filterA过滤，结果用filterB再次过滤-->
{{ message | filterA | filterB }}
```

#### 过滤器是js函数
```vue
<!--可以接受参数-->
<!--其中message作为第一个参数，arg1、arg2为后面两个参数-->
{{ message | filterA('arg1', 'arg2') }}
```
