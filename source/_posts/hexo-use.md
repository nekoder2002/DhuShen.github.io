---
title: Hexo使用教程
img: http://rx4hz3911.hd-bkt.clouddn.com/57bd443112573.jpg
date: 2023-07-02 18:00
summary: 本文介绍了hexo的基础命令
categories: 前端技术
tags:
- hexo
- node.js
---
## 快速上手

此文将告诉你如何快速上手hexo

![](http://rx4hz3911.hd-bkt.clouddn.com/v2-7827197bfa0023932d428633d41372fd_250x0.jpg)

### 创建项目

```bash
  $ npm install hexo-cli -g
  $ hexo init blog
  $ cd blog
  $ npm install
  $ hexo server
```

  

### 创建文章

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### 本地运行

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### 生产静态页面

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### 部署到GitHub Pages

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)
