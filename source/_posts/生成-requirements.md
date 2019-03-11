---
title: 项目内生成 requirements.txt
tags:
  - Python
  - requirements
  - virtualenvs
date: 2018-05-18 12:08:24
---


Python 安装类似前端自动生成依赖到 package.json
pip 安装 package 时自动生成依赖到 requirements.txt
`pip freeze > requirements.txt`

运行后按照的packages太多了，好多并没有实际在项目中用到，改用：
`pip freeze -l > requirements.txt`

再次运行会更新所有包到最新版本 ...