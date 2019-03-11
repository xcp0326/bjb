---
title: Linux 命令：cat，ls，mv，touch
date: 2018-10-09 17:08:09
tags: 
  - Linux
  - ls
  - cat
  - mv
  - touch
---
## 几个常用的 Linux 命令
回顾并记录下几个常用的 Linux 命令：ls，cat，mv，touch

### ls 命令
#### 描述
ls 是英文 list segment（列出文件块）的缩写，用于列出文件。

#### 用法
- 当 ls 命令后不加参数时，列出当前目录下的所有文件和目录名。
- 如果是以目录作为参数则会列出该目录下的文件。
- 如果以多个目录和文件作为参数时，ls 则会列出所有指定的文件和目录中的文件名。
- ls -a，all 意为显示所有文件，会列出包括 . 点，.. 点点 以及 其他以点开头的文件
- ls -l，l 是 long 意为长格式，这时则会列出文件的类型，权限，硬链接数目，文件拥有者，文件所在组，大小，日期，最后编辑时间和文件名
- ls -R，迭代显示目录下所有的子目录
  ls -R / 则会显示文件系统中所有文件
  ls -R /xxx 迭代显示文件夹 xxx 下的所有文件
- ls -tl，按照修改日期倒序排列文件

### cat 命令
#### 描述
cat 全称为 concatenate，是用来查看文件连续内容用的指令。

#### 用法
- cat file.txt，为查看 file.txt 的内容。这也是我用的最常的。head file.txt 的处理结果也是的。
- cat file1.txt > file2.txt，将 file1.txt 的内容覆盖 file2.txt。与 echo 'xxx' > file2.txt 有相同之处。
- cat file1.txt >> file2.txt，将 file1.txt 的内容续接在 file2.txt 里面，意思 file2.txt 包含了原来 file2.txt 的内容及新加入的 file1.txt 的内容。与 echo 'xxx' >> file2.txt 有相同之处。看来关键点在 '>>' 符号？待查

### mv 命令
#### 描述
mv 全称为 move，是用来移动文件或者修改文件名的指令。

#### 用法
- mv file1.txt file2.txt，修改 file1.txt 文件名为 file2.txt
- mv file1.txt file2，file1.txt 文件移动到 file2
- mv -i file1.txt file2，file2 文件夹内若有 file1.txt，则询问是否覆盖，这里的 i 为 destination 目标文件
- mv -f file1.txt file2，file2 文件夹内若有 file1.txt，则直接覆盖，这里的 f 意思为 force，强制覆盖。
- mv -v file1.txt file2, 显示移动的轨迹。v 意思是 verbose，啰嗦，冗长。

### touch 命令
#### 描述
touch 是用来修改文件或者目录的时间属性的，包括存取时间和更改时间。若文件不存在，就创建一个新的文件。

#### 用法
- touch filename，如果 filename 不存在则创建一个 filename，否则修改 filename 的更改时间。
- touch a.txt b.txt，同上，只是同时修改或者创建
- touch -r a.txt b.txt，b.txt 的时间设置为 a.txt 相同
- touch -t YYYYMMDDHHMM.SS filename，设置 filename 的时间为 YYYYMMDDHHMM.SS
- touch -c filename，如果不存在 filename 不创建

### PS
可以在这个网站 explainshell.com 上输入命令查询命令的解释。只是输入一个命令它只会显示这一个命令的描述，并不会将它的详细的选项参数相关描述显示出来。比如 ls 只会显示 list directory contents。查询 ls -a 才能显示 ls -a 的具体的描述。

