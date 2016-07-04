---
layout: post
title: Github/Gitlab多账户SSH Key切换
date: 2016-07-01 17:38:13
comments: true
categories: tool config
tags: [Git]
keywords: ssh, GitLab, GitHub
description: 使用ssh config实现GitLab 和 GitHub账户切换
---
由于目前公司使用GitLab托管代码,本人又在GitHub上有自己的代码仓库。同时用来生成public key的邮箱也不一样,公司GitLab用的是 daixu@zuche.com ,自己GitHub用的是 cynicism2011@gmail.com 。
这样用起来就非常操蛋了,每次切换的时候都得切换public key。google了一下发现用ssh config轻松解决。

### 配置Git邮箱

```bash
    #默认全局 GitHub使用
    git config --global user.name 'hughdai' && git config --global user.email 'cynicism2011@gmail.com'
    #公司项目 GitLab使用
    git config --local user.name 'daixu' && git config --local user.email 'daixu@zuche.com'
```

### 生成SSH Key

ssh有不会的话自行google

```bash
    # 默认文件名 GitHub使用
    ssh-keygen -t rsa -C 'cynicism2011@gmail.com'
    # 指定文件名 GitLab使用
    ssh-keygen -t rsa -f ~/.ssh/id_rsa.zuche -C 'daixu@zuche.com'
```
分别把.pub文件添加到SSH Keys中

### 配置ssh config
执行 touch ~/.ssh/config,创建config文件,添加相应配置

```bash
    Host *.zuche.com
         HostName zuche.com
         IdentityFile ~/.ssh/id_rsa.zuche
```

### 验证
```bash
    ssh -T git@github.com
```