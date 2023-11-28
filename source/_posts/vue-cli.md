---
title: vue-cli
date: 2023-11-28 11:02:50
tags: 
- Vue
- Project
categories:
- programming
- project
---

## Vue-Cli	

Vue Cli是一个基于Vue.js进行快速开发的完整系统

- 通过@vue/cli实现交互式项目脚手架
- 通过@vue/cli+@vue/cli-service-global实现零配置原型开发
- 运行时依赖(@vue/cli-server)
  - 可升级
  - 基于webpack，带有合理的默认配置
  - 可通过项目内的配置文件进行配置
  - 可通过插件进行扩展

- 有丰富的插件合集，集成了前端生态中最好的工具
- 有完整的图形化的创建和管理Vue.js项目和用户界面

### 安装

```shell
npm install -g @vue/cli
# or
yarn global add @vue/cli

# 版本
vue -version

# 升级
npm update -g @vue/cli
# or
yarn global upgrade --latest @vue/cli
```

### 基础

#### 创建项目

- 默认创建

```shell
vue create hello-world
```

- 增加选项

```shell
creat [options] <app-name>
```

![image-20231128130654089](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202311281306205.png)

- 使用图形化界面创建项目

```shell
vue ui
```

