---
title: Vue Api
date: 2023-11-23 15:29:03
tags: Vue
categories: 
- programming
- Vue
---

## 	API

### 全局配置

 > Vue.config是一个对象，包含Vue的全局配置
 > 包含以下属性，可以在启动应用之前修改

#### silent
- 类型：boolean
- 默认值：false
- 作用：取消Vue所有日志与警告
- 用法：
```js
Vue.config.silent = true;
```

#### optionMergeStrategies
- 类型：{[key: string]: Function}
- 默认值：{}
- 作用：自定义合并策略的选项
- 用法：
```js
// 第一个参数第二个参数为父实例和子实例，Vue实例上下文作为第三个参数传入
Vue.config.optionMergeStrategies._my_option = function(parent, child, vm){
	return child + 1;
}

const Profile = Vue.extend({
	_my_option:1
})
```

#### devtools
- 类型：boolean
- 默认值：true(生产版为 false)
- 作用：是否允许vue-devtools检查代码，开发版本默认为true，生产版本默认为false
- 用法
```js
Vue.config.devtools = true;
```

#### errorHandler
- 类型：Function
- 默认值：undefined
- 作用：指定组件的渲染和观察期间未捕获错误的处理函数，可以获取错误信息和Vue实例
- 用法
```js
Vue.config.errorHandler = function(err, vm, info){
	// handle error
	// info是Vue特定的错误信息，比如错误所在的生命周期钩子
}
```

#### warnHandler
- 类型：Function
- 默认值：undefined
- 作用：为Vue运行时警告赋予一个自定义处理函数，只在开发者模式生效
- 用法
```js
Vue.config.warnHandler = function(msg, vm, trace){
	//trace是组件的继承关系追踪
}
```

#### ignoredElements
- 类型：`Array<string|RegExp>`
- 默认值：[]
- 作用：使Vue忽略在Vue之外的自定义元素
- 用法
```js
Vue.config.ignoredElements= [
	'my-custom-web-component',
	'another-web-component'
	// 用一个RegExp忽略所有ion-开头的元素
	/^ion-/
]
```

#### keyCodes
- 类型：{[key:string]: number | `Array<number>`}
- 默认值：{}
- 作用：给v-on自定义键位别名
- 用法
```js
Vue.config.keyCodes={
	v:86,
	f1:112,
	// camelCase不可用
	mediaPlayPause:179,
	// 取而代之的是kebab-case且哟双引号括起来
	"media-paly-pause":179,
	up:[38,87]
}
```
```vue
<input type="text" @keyup.media-play-pause="func"/>
```

#### performance
- 类型：boolean
- 默认值：false
- 作用：在浏览器开发工具的性能/时间线面板中启用对组件初始化、编译、渲染和打补丁的性能追踪
- 用法
```js
Vue.config.performance = true;
```

#### productionTip
- 类型：boolean
- 默认值：true
- 作用：阻止vue在启动时生成生产提示
- 用法
```js
Vue.config.productionTip = false;
```

### 全局API

#### extend(options)
- 参数：{object} option
- 作用：全局组件
- 用法：使用id绑定元素，组件会插入到元素中间
```vue
<div id="mount-point"></div>
```
```js
// 创建构建器
var Profile = Vue.extend({
	template:'<p>hello {{msg}}</p>',
	// 此处的data特殊，为function
	data:function(){
		return{
			msg:"micle"
		}
	}
})
// 创建Profile实例，挂载到一个元素上
// 结果显示:hello micle
new Profile().$mount('#mount-point');
```

#### nextTick([callback,context])
- 参数
	- {function} [callback]
	- {object} [context]
- 作用：在下次 DOM 更新循环结束之后执行延迟回调，修改数据后立刻使用此方法，可以获取更新后的DOM
- 用法
```js
// 修改数据
vm.msg='hello'
// DOM还没有更新
Vue.nextTick(function(){
	// DOM更新了
	// 可以从这里获得更新后的数据
})

// 可以作为promise使用
Vue.nextTick()
.then(functiom{
	// DOM更新了
})
```

#### set(target,propertyName/index,value)
- 参数
	- {object|Array} target:要操作的对象
	- {string|numver} propertyName/index：添加的属性名称
	- {any} value：属性的值
- 返回值：设置的值
- 作用：在Vue实例中为响应式对象设置属性值，在添加新属性时，确保新属性也是响应式的
```js
// 在Data对象中响应式地添加newProperty属性，值为'new value'
Vue.set(this.Data,'newProperty','new value');

// 等价于
// 虽然vue会自动将下列值响应式地给newProperty属性，但是不保证新的属性是响应式的
this.Data.newProperty = 'new value'
```

#### delete(target,propertyName/index)
- 参数
	- {object|Array} target：要操作的对象
	- {string|number} propertyName/index：要删除的属性
- 作用：删除对象的property，如果对象是响应式的，确保删除也是响应式的
- 用法
```js
// 在Data对象中响应式地删除propertyName属性
Vue.delete(this.Data,'propertyName')

// 等价于
// 此方式不能保证删除是响应式的
delete this.Data.propertyName;
```

#### directive(id,[definition])
- 参数
	- {string} id
	- {Function|Object} [definition]
- 作用：注册或者获取全局指令(如vue自带的全局指令：v-if、v-on...)
- 用法
```js
Vue.directive('directiveName',{
	// 指令选项
	bind(el,binding,vnode){
		// 在元素绑定到指令时调用
	},
	// 其他钩子函数
	inserted(){},// 在子元素被插入到父元素时调用
	update(){},// 在元素所在的组件更新时调用，可能调用多次
	componentUpdate(){},// 在元素所在组件和其子组件更新后调用
	unbind(){}// 在元素解绑时调用，用于清理绑定的资源
});

// 或者注册(指令函数)
Vue.directive('directiveName',function(){
	// 这里会被bind和update调用
})

// getter,返回已注册的指令
var myDirective = Vue.directive('my-directive')
```
```vue
<!--使用自定义指令-->
<div v-directiveName="value"></div>
```

#### filter(id,[definition])
- 参数
	- {stirng} id
	- {Function} [definition]
- 作用：注册或获取全局过滤器
- 用法
```js
// 注册
Vue.filter('myFilter',function(val){
	// 返回处理后的值
})

// getter，返回已注册的过滤器
var myFilter = Vue.filter('myFilter');
```
```vue
<!--组件中使用全局过滤器-->
<div>
	<p>{{ msg | myFilter }}</p>
</div>
```

#### component(id,[definition])
- 参数
	- {stirng} id
	- {Function|Object} [definition]
- 作用：注册或获取全局组件，注册会自动使用给定的id设置组件的名称
- 用法
```js
// 注册组件，传入一个扩展过的构造器
Vue.component('my-component',Vue.extend({ /*...*/ }))

// 注册组件，传入一个选项对象(自动调用 Vue.extend)
Vue.component('my-component',{/*...*/})

// 获取注册的组件(始终返回构造器)
// 使用方式与普通组件一致
var MyComponent = Vue.component('my-component')
```

#### use(plugin)
- 参数
	- {Object | Function} plugin
- 作用：安装Vue.js插件，参数也是插件的名称，需要在new Vue()之前调用
- 用法
```js
Vue.use(plugin);
```

#### mixin(mixin)
- 参数
	- {Obejct} mixim
- 作用：全局注册一个混入，影响注册之后所有创建的每个Vue实例
- 用法
```js
// 为自定义的选项'myOption'注册一个处理器
Vue.mixin({
	created:function(){
	var myOption = this.$options.myOption
	if(myOption){
		console.log(myOption)
	}
	}
})

new Vue({
	myOption:'hello'
})
// => "hello"
```

#### compile(template)
- 参数
	- {string} template
- 作用：将一个模板字符串编译成render函数
- 用法
```js
var res = Vue.compile('<div><span>{{ msg }}</span></div>')

new Vue({
	data:{
		msg:'hello'
	},
	// 渲染<div><span>{{ msg }}</span></div>为根组件
	render:res.render,
	staticRenderFns:res.staticRenderFns
})
```

#### observable(object)
- 参数
	- {object} object
- 作用：让一个对象可响应，Vue内部会用它来处理data返回的对象
- 用法
```js
const state = Vue.observable({ count:0 })

const Demo = {
	render(h){
		return h('button',{
			on:{click:()=>{state.count++}}
		},'count is : ${ state.count }')
	}
}
```

#### version
- 细节：提供字符串形式的Vue安装版本号
- 用法
```js
var version = Number(Vue.version.split('.')[0])

if(version === 2){
	// Vue v2.x.x
}else if(version === 1){
	// Vue v1.x.x
}else{
	// Unsupported version of Vue
}
```

## 选项

### 数据

#### data
- 类型：object | function
- 限制：组件的定义只接受function
- 解释
	- data应该只是数据，不推荐观察拥有状态行为的对象
	- 通过vm.$data可以访问数据对象，但是vue实例已经代理了data对象上所有的property，因此vm.a等同于vm.$data.a

#### props
- 类型：`Array<string>` | Object
- 解释
	- 可以是数组、对象，用于接收父组件的数据
	- 设置props时可选项：
		- type:String|Number|Boolean|...
		- default:any
		- required:Boolean
		- validator:Function(自定义验证函数)
- 使用
```js
props:{
// props的名称
data:{
	type:Number,
	default:()=>(null)，
	required:true,
	// 自定义验证
	validator:function(value){
		return value>=0;
	}
	}
}
```

#### propsData
- 类型：{[key : string] : any}
- 限制：只能用于new创建的实例中
- 作用：创建实例时传递props，主要是方便测试
- 用法
```js
// 创建全局组件，创建props选项
var Comp = Vue.extend({
	props:['msg'],
	template:'<div>{{ msg }}</div>'
})

// 给创建的全局组件传递props值
var vm = new Comp({
	propsData:{
	msg:'hello'
	}
})
```

#### computed
- 类型：{[key : string] : Function | {get : Function,set : Function} }
- 作用：计算属性将被混入到Vue实例中，所有的getter和setter的this上下文自动绑定为Vue实例
- tips：**计算属性的结果会被缓存，除非依赖的响应式 property 变化才会重新计算**
- 用法
```js
 computed: {
    // 仅读取
    aDouble: function () {
      return this.a * 2
    },
    // 读取和设置
    aPlus: {
      get: function () {
        return this.a + 1
      },
      set: function (v) {
        this.a = v - 1
      }
    }
  }
```

#### method
- 类型：{[key : string] : Function}
- 作用：将被混入到method实例中，可以通过vm实例访问这些方法，方法中的this自动绑定为Vue实例
- tips：不应该使用箭头函数定义methods中的方法
- 用法
```js
method:{
	func(){
		return this.a;
	}
}
```

#### watch
- 类型：{[key : string] : string | Function | Object | Array}
- 作用：代表一个对象，键是一个需要观察的表达式，值是对应的回调函数，值也可以是方法名，或者包含选项的对象
- tips：Vue实例在实例化时会调用$watch()，遍历watch对象的每一个property
- tips:不应使用箭头函数定义watcher函数
- 用法
```js
watch:{
	// 每个选项名称都是数据变量名
	a:function(val, oldVal){
		console.log('new:%s,old:%s',val,oldVal);
	}
	// 方法名
	b:'myMethod'
}
```

### DOM

#### el
- 类型：string | Element
- 限制：只在new创建实例时生效
- 作用：
	- 提供在一个页面上已存在的DOM元素作为Vue实例的挂载目标，目标可以是CSS选择器，也可以时一个HTMLElement实例
	- 实例挂载后元素可以用vm.$el访问他
	- 实例化存在这个选项，实例会进入编译过程，否则需要显示调用vm.$mount()手动开起编译

#### template
- 类型：string
- 作用：一个字符串模板为Vue实例的标识使用，模板将替换挂载的元素，如果Vue选项中包含渲染函数，该模板会被忽略

#### render
- 类型：(createElement: () => VNode)=> VNode
- 作用：字符串模板的代替方案，可以发挥js最大的编程能力。渲染函数接受一个createElement方法作为第一个参数来创建VNode
- Tips：一般在Vue项目中main.js文件中，new一个Vue实例时通过h=>h(App)的方式指定App.vue为根组件

#### renderError
- 类型：(createElement: () => VNode，error : Error)=> VNode
- 作用：在开发环境下，render函数错误时提供另一种渲染输出


### 生命周期钩子

  ![img](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309041516020.png)

#### beforeCreate
- 类型：Function
- 作用：在实例初始化之后。进行数据侦听和事件/侦听器的配置之前的同步调用

#### created
- 类型：Function
- 作用：
	- 实例创建完成后被立即同步调用
	- 在这一步中已完成对选项的处理，数据侦听、计算属性、方法、事件/侦听器的回调函数已经被配置完毕
	- 挂载还未开始，$el还不可以使用

#### beforeMounted
- 类型：Function
- 作用：
	- 挂载开始前被调用：相关的render函数首次被调用
	- 该钩子函数在服务器端渲染期间不可被调用

#### mounted
- 类型：Function
- 作用：
	- 实例被挂载后调用，这时el被挂载时创建的$el替换
	- 如果根实例挂载到了一个文档内的元素上，当mounted被调用时vm.$el也在文档内
	- mounted不会保证所有的子组件都被挂载完成，如果希望等到整个试图都渲染完毕执行某些操作，可以在mounted内部使用vm.$nextTick

#### beforeUpdate
- 类型：Function
- 作用：
	- 在数据发生改变后，DOM被更新之前调用
	- 适合在现有的DOM将要被更新之前访问它，如：移除手动添加的事件监听器
	- 该钩子函数在服务器渲染期间不调用，只在初次渲染会在服务器端进行

#### updated
- 类型：Function
- 作用：
	- 数据更改导致虚拟DOM重新渲染和更新完毕之后被调用
	- 当此钩子被调用时，组件DOM已经更新，现在可以执行依赖于DOM的操作，但是最好使用计算属性或者watch代替
	- 不会保证所有子组件也都被重新渲染完毕，如果希望等到整个视图渲染完毕可以使用vm.$nextTick

#### activated
- 类型：Function
- 作用：
	- 被keep-alive缓存的组件激活时调用
	- 该钩子函数在服务器端渲染期间不被调用

#### deactivated
- 类型：Funtion
- 作用：
	- 被keep-alive缓存的组件失活时调用
	- 该钩子函数在服务器端渲染期间不被调用

#### beforeDestroy
- 类型：Function
- 作用：
	- 实例销毁之前调用，在这实例仍然可以使用
	- 该钩子在服务器端渲染期间不被调用

#### destroyed
- 类型：Function
- 作用：
	- 实例销毁后调用，调用此钩子函数后，对应Vue实例的所有指令都被解绑，所有事件监听移除，所有子实例也销毁
	- 该钩子在服务器端渲染期间不被调用

#### errorCaptured
- 类型：(err: Error, vm: Component, info: string) => ?boolean
- 作用：
	- 捕获一个来自后代组件的错误时被调用
	- 此钩子函数收到三个参数：错误对象、发生错误的组件实例以及包含错误来源信息的字符串
	- 此钩子返回false以阻止该错误继续向上传播

### 资源

#### directives
- 类型：Object
- 作用：包含Vue实例可用的指令哈希表

#### filters
- 类型：Object
- 作用：包含Vue实例可用过滤器的哈希表

#### components
- 类型：Object
- 作用：包含Vue实例可用组件的哈希表

### 组合

#### parent
- 类型：Vue instance
- 作用：指定已创建的实例的父实例，在两者之间建立父子关系，子实例用this.$parent访问父实例，子实例被推入父实例的$children数组中
- tips：少用此组合，主要用于应急方法

#### mixins
- 类型：Array`<Object>`
- 作用：
	- mixins选项接收一个混入对象的数组
	- 混入对象可以像正常的实例对象一样包含实例选项，这些选项最后会被合并到最终选项，合并逻辑与extend一样
	- 若混入包含created钩子，创建组件本身也有，那么两个created都会被调用
	- Mixin钩子按照传入的顺序依次调用，并在调用组件自身的钩子之前被调用
- 用法：
```js
var mixin = {
	created:function(){console.log(1)}
}

var vm = new Vue({
	created:function(){console.log(2)},
	mixins:[mixin]
})
// =>1
// =>2
```

#### extends
- 类型：Object | Function
- 作用：
	- 允许声明扩展另一个组件，可以是一个简单的选项对象或者构造函数
	- 无需使用Vue.extend
	- 主要是为了便于扩展单文件组件

- 用法
```js
var CompA = {...}

// 没有调用'Vue.extend'时继承CompA
var CompB = {
	extend:CompB
}
```

#### provide/inject
- 类型：
	- provide：Object | () => Object
	- inject：Array`<string>` | {[key : string] : string | Symbol | Object}
- 作用：
	- 这对选项需要一起使用
	- 允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在其上下游关系成立的时间始终生效
```js
// 父级提供'foo'
var Provider = {
	provide:{
		foo:'bar'
	}
	// ...
}

// 子组件注入'foo'
var Child = {
	inject:['foo']
	created(){
		console.log(this.foo)
	}
}
```

### 其他

#### name
- 类型：string
- 限制：只在作为组件选项时起作用
- 作用：允许组件模板递归调用自身，使用name选项是好习惯

#### delimiters
- 类型：Array`<string>`
- 默认值：["{{","}}"]
- 限制：只在完整构建版本中的浏览器内编译时可用
- 作用：改变纯文本插入分隔符
- 用法：
```js
new Vue({
	delimiters:['${','}']
})
// 分隔符变成ES6模板
}
```

#### functional
- 类型：boolean
- 作用：
	- 使组件无状态(data)和无实例(没有this上下文)
	- 他们用一个简单的render函数返回虚拟节点使他们渲染的代价更小

#### model
- 类型：{ prop? : string,event? : string}
- 作用：
	- 允许自定义组件在使用v-model时定制prop和event
	- 默认情况下v-model的prop是value，而event是input
	- 某些单选框复选框可能使用value prop来达到不同的目的
- 用法
```js
Vue.component('my-checkbox',{
	model:{
		prop:'checked',
		event:'change'
	},
	props:{
		//允许value属性用作别的用途
		value:String,
		// 使用checked来代替value作为prop
		checked:{
			type:Number,
			default:0
		}
	}
})
```

#### inheritAttrs
- 类型：boolean
- 默认值：true
- 作用：
	- 默认情况下，父作用域上的不被认作prop的属性绑定会回退并作为普通的属性应用在子组件的根元素上
	- 通过设置inheritAttrs为false，这个默认行为会被去掉
- 解释

![image-20230908150342899](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202309081503994.png)

#### comments
- 类型：boolean
- 默认值：false
- 限制：只在完整构建版本中的浏览器内编译时可用
- 作用：当设置为true，将会保留且渲染模板中的html注释

### 实例property

#### data
- 类型：Object

#### props
- 类型：Object

#### el
- 类型：Element
- 只读
- 作用：Vue实例使用的根DOM元素

#### options
- 类型：Object
- 只读
- 作用：用于当前实例的初始化的选项

#### parent
- 类型：Vue instance
- 只读
- 作用：父实例

#### root
- 类型：Vue instance
- 只读
- 作用：组件树的根实例，若没有父实例，则此实例是自己

#### children
- 类型：`Array<Vue instance>`
- 只读
- 作用：当前实例的直接子组件，不保证顺序，非响应式

#### slot
- 类型：{[name : string] : ?Array`<VNode>`}
- 只读
- 响应性：否
- 作用：
	- 访问被插槽分发的内容
	- 每个具名插槽的property(如v-slot:foo中的内容会在vm.$slot.foo中找到)
	- default：包括了所有没有被包含在具名插槽中的节点或者v-slot:default的内容
	- 使用渲染函数书写一个组件，访问slots很有帮助

#### scopedSlots
- 类型：{[name : string] : props=>Array`<VNode>` | undefined}
- 只读
- 作用：
	- 访问作用域插槽，包括默认slot在内的每个插槽，该对象都包含一个返回相应VNode的函数
	- 在使用渲染函数开发一个组件时很有用

#### refs
- 类型：Object
- 只读
- 作用：一个对象，持有注册过ref attribute的所有DOM元素和组件的实例

#### isServer
- 类型：boolean
- 只读
- 作用：当前Vue实例是否运行于服务器

#### attrs
- 类型：{[key : string] : string}
- 只读
- 作用：
	- 包含了父作用域中不作为prop被识别的attribute绑定
	- 当一个组件没有声明任何prop时，这里会包含所有父作用域的绑定

#### listeners
- 类型：{[key : string] : function | Array`<Function>`}
- 只读
- 作用：包含了父作用域中的v-on事件监听器，可以通过v-on= "$listeners"传入内部组件

### 实例方法/数据

#### watch(expOrFn,callback,[options])
- 参数
	- {string | Function} expOrFn
	- {Function | Obejct} callback
	- {Object} [options]
		- {boolean} deep
		- {boolean} immediate
- 返回值：{Function} unwatch
- 作用：
	- 观察Vue实例上的一个表达式或者一个函数计算结果的变化
	- 其中回调函数的参数为新值和旧值
	- 表达式只接收简单的键路径，对于更复杂的表达式使用函数替代
- 用法
```js
// 键路径
vm.$watch('a.b.c', function(newVal,oldVal){
	// ...
})

// 函数
vm.$watch(
	function(){
		// 监听的值
		// 此处为a+b的值
		return this.a + this.b
	},
	function(newVal,oldVal){
	  //
	}
)

// 取消观察函数
var unwatch = vm.$watch('a', cb)
unwatch()
```
- 选项
	- deep:对象内部的值变化
	- immediate：立即以表达式的当前值触发回调
```js
vm.$watch('a',callback,{
	deep:true,
	immediate:true
})
```

#### set(target,propertyName/index,value)
- 参数
	- {Object | Array} target
	- {string | number} propertyName/index
	- {any} value
- 返回值：设置的值
- 作用：全局Vue.set的别名

#### delete(target,propertyName/index)
- 参数
	- {Object | Array} target
	- {string | number} propertyName/index
- 作用：全局Vue.delete的别名

### 实例方法/事件

#### on(event,callback)
- 参数
	- {string | Array`<string>`} event (数组只在 2.2.0+ 中支持)
	- {Function} callback
- 作用：
	- 监听当前实例上的自定义事件
	- 事件可以由vm.$emit触发
	- 回调函数会接收所有的传入事件触发函数的额外参数
- 用法
```js
vm.$on('test',function(msg){
	console.log(msg)
})

// 触发事件
vm.$emit('test', 'hi')
```

#### once(event,callback)
- 参数：
	- {string} event
	- {Function} callback
- 作用：
	- 监听一个自定义事件
	- 只监听一次，触发之后，监听器被移除

#### off([event,callback])
- 参数
	- {string | Array`<string>`} event (只在 2.2.2+ 支持数组)
	- {Function} [callback] 
- 作用
	- 移除自定义事件监听器
		- 无参数：移除所有事件监听器
		- 只提供事件：移除该事件的所有监听器
		- 同时提供事件与回调：只移除这个回调的监听器

#### emit(eventName,[...args])
- 参数
	- {string} eventName
	- [...args]
- 作用：触发当前实例上的事件，附加的参数都会传给监听器回调
- 用法
```js
Vue.component('welcome-button', {
  template: `
    <button v-on:click="$emit('welcome')">
      Click me to be welcomed
    </button>
  `
})

new Vue({
  el: '#emit-example-simple',
  methods: {
    sayHi: function () {
      alert('Hi!')
    }
  }
})
```
```vue
<div id="emit-example-simple">
  <welcome-button v-on:welcome="sayHi"></welcome-button>
</div>
```

### 实例方法/声明周期

#### mount([elementOrSelector])
- 参数
	- {Element | string} [elementOrSelector]
	- {boolean} [hydrating]
- 返回值：vm-实例自身
- 作用：挂载实例
- 用法
```js
var myComponent = Vue.extend({
	//
})

// 挂载实例到id=app的元素
new MyComponent().$mount('#app')
```

#### forceUpdate()
- 作用：迫使Vue实例重新渲染，仅影响实例本身和插入插槽内容的子组件，不是所有子组件

#### nextTick([callback])
- 参数：{Function} [callback]
- 作用：
	- 将回调延迟到下次DOM更新循环之后执行
	- 在修改数据之后立即使用它，然后等待DOM更新
	- 与全局nextTick一样，不同的是回调的this自动绑定到调用它的实例

#### destroy()
- 作用：
	- 完全销毁一个实例
	- 清除它与其他实例的连接，解绑它的全部指令以及事件监听器
	- 触发beforeDestroy和destroyed钩子

### 指令
