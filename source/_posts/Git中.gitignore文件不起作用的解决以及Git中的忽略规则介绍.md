---
title: Git中.gitignore文件不起作用的解决以及Git中的忽略规则介绍
date: 2018-11-21
updated: 2018-11-21
tags: Git
categories: 版本控制
keywords: 
description: 
top_img: /img/git.jpg
cover: /img/git.jpg
---

在IDEA里使用Git管理代码的过程中，可以修改.gitignore文件中的标示的方法来忽略开发者想忽略掉的文件或目录，如果没有.gitignore文件，可以自己手工创建。在.gitignore文件中的每一行保存一个匹配的规则例如：

``` txt
# 此为注释 – 将被 Git 忽略

*.a       # 忽略所有 .a 结尾的文件
!lib.a    # 但 lib.a 除外
/TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/    # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```

在填写忽略文件的过程中，我发现在IDEA里面，.gitignore中已经标明忽略的文件目录下的文件.

当我想git push的时候还会出现在push的目录中，原因是因为在Studio的git忽略目录中，新建的文件在git中会有缓存，如果某些文件已经被纳入了版本管理中。

就算是在.gitignore中已经声明了忽略路径也是不起作用的，这时候我们就应该先把本地缓存删除，然后再进行git的push，这样就不会出现忽略的文件了。

git清除本地缓存命令如下：

``` shell
git rm -r --cached . && git add . && git commit -m 'update .gitignore' && git push
```


Git强制覆盖服务器端代码
``` shell
git push origin master --force
```

