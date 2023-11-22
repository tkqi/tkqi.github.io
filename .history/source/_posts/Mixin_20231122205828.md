---
title: Mixin
date: 2023-11-22 10:16:49
tags: Vue
categories: 
- programming
- Vue
---

## 混入

### 基础
- 混入是一种灵活的复用功能的方式
- 一个混入对象可以包含任意组件选项
- 所有混入对象的选项被混合进该组件本身的选项

```js
// 定义混入对象
var myMixin = {
	created:function(){
		this.hello()
	},
	methods:{
		hello:function(){
			console.log('hello from mixin!')
		}
	}
}

// 定义一个使用混入对象的组件
var Component = Vue.extend({
	mixins:[myMixin]
})

Var Component = new Component() // =>hello from mixin!
```
### 选项合并
- 当组件和混入对象有同名选项时，这些选项以恰当的方式合并
- 数据对象会递归合并，发生冲突以组件数据优先

```js
var mixin ={
	data:function(){
		return{
			message:'hello',
			foo:'abc'
		}
	}
}

new Vue({
	mixins:[mixin],
	data:function(){
		return{
			message:'goodBye',
			bar:'def'
		}
	},
	created:function(){
		console.log(this.$data)
		// => {message:'goodBye',foo:'abc',bar:'def'}
	}
})
```
- 同名的钩子函数合并为一个数组，都将被调用
- 混入对象的钩子在组件自身的钩子之前被调用
- 值为对象选项时，如methods、components和directives将被合并为同一个
对象
- 值为对象选项时，两个对象键名冲突，去组件对象的键值对

### 全局混入
- 混入可以全局注册
- 全局使用混入需要小心，因为它会影响所有组件实例
```js
// 为自定义的选项'myOption'注入一个处理器
Var mixin({
	created:function(){
		var myOption(){
			if(myOption){
				console.log(myOption)
			}
		}
	}
})

new Vue({
	myOption :'hello'
})
```


### 自定义选项合并策略
