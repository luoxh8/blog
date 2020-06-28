---
title: Git常用
date: 2018-11-21
updated: 2018-11-21
tags: Git
categories: 版本控制
keywords: 
description: 
top_img: /img/git.jpg
cover: /img/git.jpg
---



快速推送：

``` shell
git add .
git commit -m "quick commit $(date "+%Y-%m-%d %H:%M:%S")"
git push
```



.gitignore不生效：

``` shell
git rm -r --cached .
git add .
git commit -m "quick update .gitignore $(date "+%Y-%m-%d %H:%M:%S")"
git push
```



git强制覆盖服务器端代码（master分支）

``` shell
git push origin master --force
```



git强制覆盖本地代码（master分支）

```shell
git fetch --all
git reset --hard origin/master
git pull
```

