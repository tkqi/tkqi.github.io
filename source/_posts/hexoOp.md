---
title: hexo
date: 2023-11-30 09:58:43
tags: 
- hexo
categories:
- tools
---

## hexo操作

### 一般命令

- init

  ```shell
  hexo init [folder]
  ```

  

  - 新建一个网站，如未设置folder则默认再目前文件夹建立网址

- new

  ```shell
  hexo new [layout] <title>
  
  # 指定文章路径
  # 路径起始位置是_posts
  # 创建文件的位置为_posts/javaScript/test.md 
  # 标题为filename
  hexo new <filename> -p javaScript/test
  ```

  

  - 新建一篇文章，如未设置layout，默认使用_config.yml中的default_layout参数代替
  - 参数：
    - -p,--path：自定义新文章路径
    - -r,--replace：如存在同名文章，将其替换
    - -s,-slug：文章的slug，作为新文章的文件名和发布后的URL

- generate

  ```shell
  hexo generate
  
  # 简写
  hexo g
  ```

  - 生产静态文件

- publish

  ```shell
  hexo publish [layout] <filename>
  ```

  

  - 发表草稿

- server

  ```shell
  hexo server
  ```

  

  - 启动服务器，默认端口4000

- clean

  ```shell
  hexo clean
  ```

  

  - 清除缓存文件和以生产的静态文件
  - 某些情况下(更换主体后)，更改对网站无效，就需要清除后重新生成静态文件等操作

### 生成静态网站

- 在本地默认的4000端口预览网站

```shell
# 生成静态网页
hexo generate
# 运行静态网页
hexo server
```

