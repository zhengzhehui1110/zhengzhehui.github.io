title: Hexo Fluid添加tag-cloud插件
tags:

  - Hexo
  - 前端
categories:
  - 前端
  - Hexo
date: 2021-02-25 20:29:00
---


## 前言
tag-cloud插件比较特殊，根据官方文档，它需要将一段代码替换原本的标签云。而标签云的代码位置可能会根据主题、模板引擎的不同而有较大差异。

虽然Fluid不算冷门的主题，但是看了若干技术博客，并没有Fluid+tag-cloud的相关教程，官方文档给出的代码替换方式也和我的实际操作有所不同，故写此篇博客以供参考

- hexo v5.0
- npm安装所有插件

## 确认模板引擎



### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)