---
title: Mac 下安装 virtualenv
tags:
  - virtualenv
  - python
date: 2018-05-18 12:06:06
---

### 第一步 pip 运行安装：

```
sudo pip install virtualenv
sudo pip install virtualenvwrapper
```

如果报错：
matplotlib 1.3.1 requires nose, which is not installed.
matplotlib 1.3.1 requires tornado, which is not installed.
如果不想安装 matplotlib，可以：
```
sudo -H pip install --ignore-install matplotlib virtualenv
sudo -H pip install --ignore-install matplotlib virtualenvwrapper
```

### 第二步 找到virtaulenvwrapper.sh的位置
```
which virtualenvwrapper.sh
```

### 第三步 修改bash启动文件.bash_profile
在.bash_profile的最后加入：
```
export WORKON_HOME=$HOME/.virtaulenvs
source /user/local/bin/virtualenvwrapper.sh
```

### 题外：
如果bash安装了oh my zsh，把上面俩行代码写入到 .zshrc
- .bash_profile 是用户登录会被读的
- .bashrc 是用户不登录也会被读的
- .zshrc 是用户登录、不登录都会被读的


## 参考链接：
[Mac 下安装 virtualenv](https://www.binss.me/blog/install-virtualenv-and-virtualenvwrapper-on-mac/)
[.bashrc、.bash_profile 和 .zshrc](https://ruby-china.org/topics/20381)
