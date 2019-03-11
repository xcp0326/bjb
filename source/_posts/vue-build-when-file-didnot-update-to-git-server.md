---
title: 没有提交 git，vue 无法打包成功。[无法重现]
date: 2017-12-14 14:33:45
tags: 
  - vue
  - build
  - vscode
  - rsync
---

遇到一个奇怪的问题
为了方便开发
我在本地开发 vue 时，用 rsync 一类的 vscode 插件同步代码到服务器了
再在服务上直接 npm run build
不知道为啥，执行的时候，rsync 同步过来的文件都没有被成功打包  

[2017-12-16 更新]
汗了，又无法重现了...