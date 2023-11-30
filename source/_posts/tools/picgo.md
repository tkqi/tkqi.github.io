---
title: picgo
date: 2023-11-20 19:48:11
tags: pictures
categories: tools
---

## typora & picgo

- typora：md编辑器，好用爱用
- picgo：图片上传软件，可配置图床，简单好用

### github配置

- 新建仓库，勾选新建时生成readme文件

- 打开GitHub设置 -> Developer Settings 

  ![image-20231120200000909](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202311202000950.png)

- -> Personal access tokens

- -> Tokens，获取token

  ![image-20231120195848839](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202311201958940.png)

### picgo配置

- 安装picgo：https://github.com/Molunerfinn/PicGo
- 图床配置(使用github)：
  - 仓库名：即github创建的仓库名
  - 分支名：默认main
  - token：在github开发者设置中获取过的token
  - 存储路径：默认img/
  - 域名：使用默认值

![image-20231120200715034](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202311202007077.png)

- 设置：在picgo设置中将时间戳重命名打开

### typora配置

- 插入图片时选择上传图片，勾选对本地、网络图片使用此规则
- 上传服务
  - 选择picgo
  - 路径选择picgo下载的exe路径
  - 可进行验证上传，出现上传成功提示则配置完成

![image-20231120201001336](https://raw.githubusercontent.com/tkqi/myMarkdownPicture/main/img/202311202010392.png)

### boom

> 获得一个可以可上传图片的无敌的md编辑器
