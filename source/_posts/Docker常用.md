---
title: Docker常用
date: 2019-02-02
updated: 2019-03-15
tags: 运维
categories: 后端
keywords: 
description: 
top_img: /img/docker.jpeg
cover: /img/docker.jpeg
---

###  docker

重新打包新镜像

```shell script
docker build -t docker001:1.0.2 .
```

启动一个新的容器

```shell script
docker run -d \
-p 8123:80 \
-v /root/test:/docker001 \
--name=docker001 \
docker001:1.0.2
```

启动一个新的容器测试

```shell script
docker run -it \
-v /root/test:/docker001 \
-p 8333:80 \
docker001:1.0.2 bash
```

停止所有容器，以及删除所有容器

``` shell
// 停止所有容器
docker stop $(docker ps -a -q) 
// 删除所有容器
docker  rm $(docker ps -a -q) 
// 删除没有名字的镜像
docker rmi $(docker images -f "dangling=true" -q)
```

