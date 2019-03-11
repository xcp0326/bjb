---
title: linux 命令行图片翻转
date: 2017-12-20 12:40:00
tags: [linux, 图片, imagemagick]
---
胖达的网站里面的图片都是我本地直接 rysnc iPhone 手机里的照片到阿里云的。
结果好多图片都是横着的。

好，找到这个：
ImageMagick 图片翻转
convert example.jpg -rotate 90 example-rotated.jpg

[Linux图片软件](https://linux.cn/article-21-1.html)安装了 gpicview，这个软件应该是在 Linux GUI 里面用的。
[移除 apt-get remove --purge gpicview](http://blog.csdn.net/get_set/article/details/51276609)

最后居然还是 ImageMagick

安排 ImageMagick 问题：
- display.im6: unable to open X server `' @ error/display.c/DisplayImageCommand/428. 没找到解决的办法

转移另一个方案：
虽然图片翻转了，可是 hexo 里面的图片样式的设置也是个麻烦的事。iPhone 直接 rsync 的图片也有点大了，每次请求都好慢。（还是想再研究下ImageMagick的其他方法，也许能找到合适的。）
转战本地修饰好了，改好宽高，压缩了大小，美化了照片再上传。

2017-12-21 updated
照片是在太大了，随便都是几兆。压缩下

参考链接：
- [Linux下图像压缩、格式转换、缩放、旋转](http://blog.csdn.net/zrools/article/details/51347471)
- [ImageMagick](https://www.imagemagick.org/script/index.php)
- [How can I open an image file from the Linux terminal?](https://www.computerhope.com/issues/ch001720.htm)
- [mac 命令行批量处理照片](http://www.jianshu.com/p/05bdcbe320f6)