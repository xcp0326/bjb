---
title: Git 笔记
date: 2017-12-19 23:22:30
tags: [git]
---
### git commit 时，文件名中文是乱码：
git config --global core.quotepath false 
如果是true：输入路径名大于0x80的字符串，转义并用双引号印起来，否则不转义。[Git帮助文档查看详情](https://www.git-scm.com/docs/git-config#git-config-corequotePath)

### git pull 时候冲突。
俩份代码的commit没有在同一条线上时，就会有这个冲突 fatal: refusing to merge unrelated histories
git pull origin master --allow-unrelated-histories