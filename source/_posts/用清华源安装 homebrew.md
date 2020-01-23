---
title: 用清华源安装homebrew
date: 2019-01-02
updated: 2019-01-02
tags: mac必备软件
categories: mac必备软件
keywords: Homebrew
description: 
top_img: /img/homebrew-logo.png
cover: /img/homebrew-logo.png
---

## 问题

homebrew 是mac 系统的包管理器，类似于 ubuntu 的 apt-get，centos 的 yum，python 的 pip。
但是直接用 homebrew 官网的方式安装，在国内会非常慢，几乎导致homebrew难以使用。
解决方法是先获取 homebrew 的安装文件 `brew_install`，并替换该文件中的安装源地址。再使用该 `brew_install`文件安装就很快。

## 获取 brew_install 文件



```bash
cd ~
curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install >> brew_install
```

## 编辑 brew_install 文件

```ruby
#!/System/Library/Frameworks/Ruby.framework/Versions/Current/usr/bin/ruby
# This script installs to /usr/local only. To install elsewhere you can just
# untar https://github.com/Homebrew/brew/tarball/master anywhere you like or
# change the value of HOMEBREW_PREFIX.
HOMEBREW_PREFIX = "/usr/local".freeze
HOMEBREW_REPOSITORY = "/usr/local/Homebrew".freeze
HOMEBREW_CACHE = "#{ENV["HOME"]}/Library/Caches/Homebrew".freeze
HOMEBREW_OLD_CACHE = "/Library/Caches/Homebrew".freeze
# BREW_REPO = "https://github.com/Homebrew/brew".freeze
# CORE_TAP_REPO = "https://github.com/Homebrew/homebrew-core".freeze
BREW_REPO = "https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git".freeze
CORE_TAP_REPO = "https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git".freeze
```

（文件后面内容省略没显示）

## 使用 brew_install 文件安装 homebrew



```bash
/usr/bin/ruby ~/brew_install
```

## 把 homebrew 替换成清华的源

在上面的安装完成之后，就可以更换 homebrew 的索引源和二进制包 bottles 源。

### 1 替换索引的镜像内容（update更新的内容）



```bash
cd "$(brew --repo)"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git

cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

brew update
```

### 2 替换 homebrew 二进制预编译包的镜像

对于 bash shell 命令行用户

```bash
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
```



对于 zsh shell:

```bash
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```