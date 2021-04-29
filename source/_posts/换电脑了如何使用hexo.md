---
title: 换电脑了如何使用hexo
date: 2020-12-24 21:19:51
tags: ["hexo"]
categories: ["Linux"]
---

## 前言

我们知道，使用 Github+hexo 搭建一个个人博客确实需要花不少时间的，我们搭好博客后使用的挺好，但是如果我们有一天电脑突然坏了，或者换了系统，那么我们怎么使用 hexo 再发布文章到个人博客呢？

如果我们还是按照之间我们总结的教程再次搭建一个博客，然后修改代码更换 hexo 主题等，各种配置特别繁琐，那么有没有一种方便的方法，直接使用我们之前搭建好的博客的源文件呢？

## 操作步骤

### 一、安装必要软件

安装 Git 

安装 node JS

<!-- more -->

### 二、源文件拷贝

将你原来电脑上个人博客目录下必要文件拷到你的新电脑上（比如F:/Blog目录下），注意无需拷全部，只拷如下几个目录：

```
_config.yml package.json scaffolds/ source/ themes/
```

### 三、安装 hexo

在 cmd 下输入下面指令安装 hexo：

```
npm install hexo-cli -g
```

### 四、进入 F:/Blog 目录（你拷贝到新电脑的目录），输入下面指令安装相关模块

```
npm installnpm install hexo-deployer-git --save  // 文章部署到 git 的模块（下面为选择安装）
npm install hexo-generator-feed --save  // 建立 RSS 订阅
npm install hexo-generator-sitemap --save // 建立站点地图
```

### 五、测试

这时候使用 `hexo s` 基本可以看到你新添加的文章了。

### 六、部署发布文章

```
hexo clean   // 清除缓存 网页正常情况下可以忽略此条命令
hexo g       // 生成静态网页
hexo d       // 开始部署
```

###  七、Github 添加 SSH Keys

 首先在本地创建 `SSH Keys`:

$ ssh-keygen -t rsa -C "user.email"

后面的邮箱即为 github 注册邮箱，也是你登录 Github 的邮箱，之后会要求确认路径和输入密码，一路回车就行。

成功的话会在 `~/`下生成 `.ssh`文件夹，进去，打开 `id_rsa.pub`，复制里面的`key`即可。