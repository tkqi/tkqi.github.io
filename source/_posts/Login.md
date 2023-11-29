---
title: Login
date: 2023-11-28 14:11:25
tags: 
- Vue
- Project
categories:
- programming
- project
- bookmarket
---

## 登陆页面逻辑
- 根路由
- 使用windows.sessionStorage
- 使用.env和config
- 背景图片如何设置

### 跟路由配置
- 初始渲染路径时'/'根目录
- 通过路由将根目录的组件设置为主页

![image-20231129104024062](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202311291040172.png)

- routes.js   --路由路径信息配置
```js
import Main from '../views/Main.vue';
import Login from '../views/loginPage.vue'
export default [
  {
    path: '/login',
    name: 'login',
    meta: { title: "登录" },
    component: Login
  },
  {
    path: '/',
    name: 'home',
    meta: { title: "主页" },
    redirect:'/home', 
    component: Main,
    children: [
      {
        path: '/home',
        name: 'Home',
        meta: { title: "主页" },
        component:()=> import('@/views/Main.vue')
      },
      {
        path: '/doc',
        name: 'Doc',
        meta: { title: "文档" },
        component:()=> import('@/views/Doc.vue')
      }
    ]
  }
]
```

- index.js   --路由参数配置，路由守卫等配置等
```js
/* eslint-disable no-empty */
/* eslint-disable no-unused-vars */
import Vue from 'vue'
import VueRouter from 'vue-router'
import baseRoutes from './routes'
Vue.use(VueRouter)

// 新建路由
const router = new VueRouter({
  mode: 'hash',
  base: process.env.BASE_URL,
  routes: baseRoutes
})

// 路由守卫
router.beforeEach((to, from, next) => {
  // 验证登录token，不在登录页则验证，否则不验证
  if (to.path == '/login') {
    return next();
  }
  const token = sessionStorage.getItem('admin_token');
  if(!token)
     return next('/login');
  else
     next();
})

// 跳转完成更新标题
router.afterEach((to, from)=>{
   document.title = to.meta.title;
})
export default router
```

### sessionStorage使用
- 可以在浏览器中存储一个键值对
- 页面关闭时将会删除该存储信息
- 用作简单存储密码

```js
// 存储数据
sessionStorage.setItem("key", "value");

// 取出数据，用'key'取出'value'
let val = sessionStorage.getItem('key');

// 移除存储的数据
sessionStorage.removeItem('key');
// 移除所有存储的键值对
sessionStorage.clear();
```

### vue项目中.env文件使用
- 存储全局环境变量
- 存储利于项目开发的vue启动打包命令

### 设置背景图
- 问题 : background-image直接赋值路径字符串不渲染背景
- 解决：    
```css
background-image: url("../assets/loginback.png");
```

## 主页面布局问题
- header -- 顶部菜单
- sidebar -- 左侧路由菜单
- hamburger -- 面包屑
- main -- 主体区域
- footer -- 底部区域

![image-20231129104937161](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202311291049245.png)

### 布局不满整个页面问题
- 直接在vue项目的template模块中写div标签
- 设置宽高100%不能铺满整个页面
- 忽视了html、body元素没有设置宽高的问题

> 解决：在App.vue中将html和body的样式宽高均设置为100%
```css
html,
body{
	height:100%;
	width:100%;
}
```

### el-container布局问题
- 引入el-container时设置自带的css无效
- 忽视了elementui的样式需要单独引入

> 解决：在main.js同时引入elementui的样式
```js
import 'element-ui/lib/theme-chalk/index.css';
```