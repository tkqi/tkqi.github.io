---
title: env
date: 2023-11-29 11:18:06
tags:
- Vue
categories:
- programming
- Vue
---

## Vue项目中的.env

- 存储全局环境变量
- 存储利于项目开发的vue启动打包命令

### 作用

可以存储不同环境下的变量

- .env	--	全局默认配置文件，不论什么环境都会加载并合并
- .env.development    --    开发环境下的配置文件
- .env.production    --    生产环境下的配置文件(正式环境)
- 可自定义.env.test/.env.beta，测试/内部测试环境下的配置文件

### 配置

变量必须以'VUE_APP_'开头

```shell
# 路由
VUE_APP_BASE_URL = '/'
```

### 启动配置

![image-20231129113418883](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202311291134926.png)

- 修改属性名称可以更改启动命令
- npm run dev   -启动->   vue-cli-service serve

### 获取.env中的全局变量
```js
let url = process.env.VUE_APP_BASE_URL
```