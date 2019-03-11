---
title: Git 的初步用法
date: 2018-10-09 17:37:00
tags: Git
---

### 第一步： 新建文件夹 test，在 test 输入 git init 指令初始化，把 test目录 作为本地仓库，初始化成功，test 下多了一个 .git 目录，有 git 相关的配置。
 ![image.png](//video.jirengu.com/xdml/image/b2221d8d-3afd-4e80-bc6a-37aabd710ed1/2018-10-9-11-26-23.png)

### 第二步：touch a.txt 新建 a.txt 文件，git add . 把 a.txt 添加到暂存区
![image.png](//video.jirengu.com/xdml/image/b2221d8d-3afd-4e80-bc6a-37aabd710ed1/2018-10-9-11-28-27.png)

### 第三步：git commit -v 用 verbose 模式 打开 vim 编辑 commit message，把 a.txt 的修改添加到本地仓库
![image.png](//video.jirengu.com/xdml/image/b2221d8d-3afd-4e80-bc6a-37aabd710ed1/2018-10-9-11-32-13.png)
