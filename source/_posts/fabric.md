---
title: Fabric 使用
tags:
  - Fabric
  - Python
  - 自动部署
date: 2018-01-03 14:48:03
---

### 安装
pip install fabric

### Start
- 新建fabfile.py
touch fabfile.py
```python
  def hello(name='world'):
      print('Hello %s' % name)
```
- 执行fab
```
fab hello:dsphoebe
out: Hello dsphoebe

     Done.
```
### 总结
- 完全python语法
- 省了```if __name__ == "__main__"```
- local测试本地
- settings修改env变量
- 失败的处理
- 其他api：prefix，run，sudo等

### 参考链接：
- [概览 & 教程](http://fabric-chs.readthedocs.io/zh_CN/chs/tutorial.html)
- [项目代码](https://github.com/dsphoebe/flask-fabric)
