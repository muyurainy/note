---
title: ubuntu下vps安装配置
date: 2018-12-17 17:41:53
tags: [vps, linux]
category: 
- 折腾
toc: true
---
> 年末vps又要到期了，最近入手了一个[Hostmybytes](http://www.hostmybytes.com/)的特价vps，只需要年付7美刀，2G内存+50GSSD+OpenVZ。目前打算搭建网站并作为辅助的代理服务器使用。

## 购买
购买链接：[https://clients.hostmybytes.com/cart.php?a=confproduct&i=0](https://clients.hostmybytes.com/cart.php?a=confproduct&i=0)，现在只需要年付7刀。虽然官网说有亚洲线路优化，但是并没有感受到太大的区别，而且刚买第一天貌似机房那边就有一点问题，所以购买要慎重。

## 修改ssh端口并禁止root登录
为了安全考虑，修改ssh默认的22端口，修改`/etc/ssh/sshd_config`:
```python
Port 22
```
禁止root登录，修改`/etc/ssh/sshd_config`：
```python
PermitRootLogin no
```

## 更新ubuntu软件源
复制以下内容到`/etc/apt/sources.list`:
```python
#deb cdrom:[Ubuntu 16.04.1 LTS _Xenial Xerus_ - Release amd64 (20160719)]/ xenial main restricted

# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
# newer versions of the distribution.
deb http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted

## Major bug fix updates produced after the final release of the
## distribution.
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-updates main restricted
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-updates main restricted

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team, and may not be under a free licence. Please satisfy yourself as to
## your rights to use the software. Also, please note that software in
## universe WILL NOT receive any review or updates from the Ubuntu security
## team.
deb http://cn.archive.ubuntu.com/ubuntu/ xenial universe
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial universe
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-updates universe
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-updates universeS
```
运行：
```python
sudo apt-get update
```
## 配置vim和tmux
- vim：上传自己的vim配置文件
- tmux：安装tmux：
    ```python
    sudo apt-get install tmux
    ```
    上传自己的配置文件

## 代理服务器安装配置
- shadowsocks：见 {% post_link vps安装配置 vps安装配置 %}
- softetherVPN：见 {% post_link vps安装配置 vps安装配置 %}

## hexo安装配置
见 {% post_link 搭建hexo博客 搭建hexo博客 %} 