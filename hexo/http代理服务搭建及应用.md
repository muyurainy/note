---
title: http代理服务搭建及应用
date: 2018-11-30 11:00:00
tags: [vps, linux, python]
category: 
- 折腾
toc: true
---
## 前言 ##
> 最近想重拾博客的更新，但是又不知道从何下手。正巧碰到一个问题就是：实验室的服务器没有联网，每次安装python包都需要下载好后传上去的话又比较麻烦而且有些包的依赖也需要装，这样会非常麻烦，所以考虑在内网的另一台机器上搭一个简单的http代理服务，然后直接走代理会比较方便一点。所以写一篇短的博客记录一下。

## 代理服务选择 ##
主要考虑的有两个，一个是[squid](http://www.squid-cache.org/)，一个是[tinyproxy](https://tinyproxy.github.io/)。squid代理不知道为什么pip老是有问题，故放弃了。然后tinyproxy一次就成功了，过程比较简单但是也记录一下。


## tinyproxy 搭建 ##
- Ubuntu只需要执行：
    ```
    sudo apt-get install tinyproxy
    ```
- 配置  
    - 配置文件的目录在`/etc/tinyproxy.conf`。
    - 修改配置：
        ```
        Port 8888 # 修改代理端口
        Allow 127.0.0.1 # 修改允许ip，要想任何ip通过，注释这句
        ```

- 启动
    ```
    service tinyproxy start
    ```

## 测试 ##
- curl: 在内网的机器中输入：
    ```
    export http_proxy=http://ip:port
    curl www.baidu.com
    ```
- pip使用代理：在内网的机器中输入：
    ```
    export https_proxy=http://ip:port
    pip install numpy
    ```
