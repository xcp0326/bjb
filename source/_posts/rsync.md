---
title: rsync 同步
tags:
  - rsync
  - 权限
  - linux
date: 2018-01-02 15:49:05
---

- -v 显示 rsync 运行的日志
- --rsync-path 指定远程机器上要运行的程序以启动远程 rsync 进程
    - 所以把 nginx 配置挪到aa服务器的 rsync 命令是：
    ```
    rsync -v --rsync-path="sudo rsync" file.conf aa:/etc/nginx/sites-available/
    ```

### 参考链接
- [Rsync 中文手册](http://www.cnblogs.com/f-ck-need-u/p/7221713.html)