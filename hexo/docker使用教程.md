---
title: docker实战
date: 2017-03-26 17:41:53
tags: [vps, linux, docker]
category: 
- 折腾
toc: true
---
> 在好多地方看到docker这一利器，但是一直没有时间学习，最近饱受多台服务器环境配置的折磨，突然来了兴致想要学习一下。
- 参考资料：[https://yeasy.gitbooks.io/docker_practice/content/](https://yeasy.gitbooks.io/docker_practice/content/)

# 实战一：Windows下ubuntu容器的使用
> 一直想要找到一种在windows下高效使用linux系统的方式，所以想到用docker试一试，同时也可以学习一下docker的使用。
## Windows下安装docker
点击以下链接下载 [Stable](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe) 或 [Edge](https://download.docker.com/win/edge/Docker%20for%20Windows%20Installer.exe) 版本的 Docker for Windows。  
双击exe文件安装即可
## 使用Dockerfile定制镜像
> 官方提供的ubuntu镜像对于日常使用是不够的，所以需要自己定制一些软件，因此打算从Dockerfile定制镜像。

在空白目录下新建Dockerfile，写入以下内容：
```bash
FROM ubuntu:latest

RUN apt-get update \
    && apt-get install -y python \
    && apt-get install -y python3 \
    && apt-get install -y python3-pip \
    && apt-get install -y python-pip python-dev build-essential \
    && apt-get install -y vim \
    && apt-get install -y net-tools \
    && apt-get install -y git
```
在Dockerfile文件所在目录执行：
```bash
docker build -t ubuntu:v1 .
```
## 启动容器
执行
```bash
docker run -it --mount type=bind,source=/d/lcy/docker,target=/root/data ubuntu:v1 bash
```
其中```-i```表示让容器的标准输入保持打开，```-t```表示让Docker分配一个伪终端，```--mount```用来挂在主机目录到容器里去。
如果遇到报错：
```bash
the input device is not a TTY.  If you are using mintty, try prefixing the command with 'winpty'
```
是由于使用了mintty，改为执行：
```bash
winpty docker run -it --mount type=bind,source=/d/lcy/docker,target=/root/data ubuntu:v1 bash
```
## 操作容器
- 启动已终止容器：
    ```bash
    docker container start 01290bcaf86d
    ```
- 进入容器
    ```bash
    docker exec -it 01290bcaf86d bash
    ```
- 终止容器
    ```bash
    docker container stop 01290bcaf86d
    ```
- 删除容器
    ```bash
    docker container stop 01290bcaf86d
    ```
- 删除所有容器
    ```bash
    docker rm $(docker ps -aq)
    ```

