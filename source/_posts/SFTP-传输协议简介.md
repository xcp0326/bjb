---
title: SFTP 传输协议简介
tags:
  - Linux
  - SFTP
date: 2018-11-06 18:50:03
---


## 什么是 SFTP

SFTP SSH 文件传输协议，与普通的文本传输协议 FTP 相比，SFTP 传输过程中用了加密/解密技术，效率比 FTP 传输要低得多。

**它的优势是可以利用安全的连接传输文件**，**还能遍历本地和远端系统上的文件**。

## 登录服务器

``` linux
sftp username@remote_host_name_or_IP
```

`!` 或 `exit` 或 `bye` 退出 SFTP

## 基本操作

登录远端服务器后，这些 SFTP 命令 ～

| 命令                | 描述                            |
| ------------------- | ------------------------------- |
| pwd                 | 查看远端当前目录         |
| lpwd                | 查看本地当前目录         |
| ls            | 查看远端当前目录下的文件 |
| lls | 查看本地当前目录下的文件 |
| cd            | 跳转到远端某个目录       |
| lcd | 跳转到本地某个目录 |
| mkdir      | 创建远端目录             |
| lmkdir | 创建本地目录 |
| rm          | 删除远端文件           |
| rmdir | 删除远端文件夹 |
| chmod、chown、chgrp | 修改文件权限                  |

远端命令前加小写字母 `l` 就是本地的命令。在 put / get 传输完文件，`lls`, `ls` 切换查看本地和远端文件传输情况。

## 传输文件到远端

```
put local_file_name
put -r local_dir
reput local_file_name // 续传，帅
```

## 下载文件到本地

```
get remote_file_name
get -r remote_dir
reget remote_file_name // 续传，接着下载，好帅呀
```

基本上上面几个命令就够平常的操作了。