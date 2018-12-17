---
title: 搭建hexo博客
date: 2017-03-29 17:41:53
tags: [vps, linux, hexo]
category: 
- 折腾
toc: true
mathjax: true
---

# 客户端安装 #
## node.js 安装 ##
- linux:   
直接去[官网下载](https://nodejs.org/en/)  
进入源码所在文件夹，并执行：
```
sudo ./configure
sudo make
sudo make install
```
或者使用[hexo官网](https://hexo.io/docs/)提供的方法
- windows:  
对于windows用户来说，建议使用安装程序进行安装,[官方下载地址](https://nodejs.org/zh-cn/)。安装时，请勾选Add to PATH选项。
另外，您也可以使用Git Bash，这是git for windows自带的一组程序，提供了Linux风格的shell，在该环境下，您可以直接用[hexo官网](https://hexo.io/docs/)提到的命令来安装Node.js。  

## git 安装 ##  

Linux (Ubuntu, Debian):

    sudo apt-get install git-core  

Linux (Fedora, Red Hat, CentOS): 

    sudo yum install git-core  

Windows:  下载并安装[git](https://git-scm.com/download/win)  

# hexo 安装配置 #
## hexo 安装 ##
Once all the requirements are installed, you can install Hexo with npm.

    npm install -g hexo-cli
## hexo 配置 ##
### 1. setup ###
Once Hexo is installed, run the following commands to initialise Hexo in the target <folder>.
```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
Once initialised, here’s what your project folder will look like:
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
具体内容参见[官网](http:\\hexo.io)
# 服务器端安装配置 #
以下是三种服务器的配置方式，分别是将hexo配置到github上、配置到vps上和利用github-webhook自动部署。
## 搭建github博客 ##
这一节是将hexo搭建到github上，下一节是将hexo搭建到vps上，请根据需求选择
### 1. 修改网站相关信息 ###
打开_config.yml
```
title: inerdstack
subtitle: the stack of it nerds
description: start from zero
author: inerdstack
language: zh-CN
timezone: Asia/Shanghai
```
language和timezone都是有输入规范的，详细可参考[语言规范](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)和[时区规范](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)。

注意：每一项的填写，其:后面都要保留一个空格，下同。

### 2. 配置统一资源定位符（个人域名） ###

    url: http://inerdstack.com

对于root（根目录）、permalink（永久链接）、permalink_defaults（默认永久链接）等其他信息保持默认。
### 3. 配置部署 ###
```sh
deploy:
  type: git
  repo: https://github.com/yourname/yourname.github.io.git
  branch: master
```
### 4. 运行 ###
    hexo server
然后打开浏览器测试
### 5. 发布到github ###
执行  
```
$ hexo generate
$ npm install hexo-deployer-git --save
$ hexo deploy
```
完毕！
## 在vps上部署hexo ##
### 创建hexo用户 ###
使用root权限添加用户，修改密码。
```
# adduser hexo
# passwd hexo
```
生成公钥并复制到用户hexo下

    ssh-keygen -t rsa
    mkdir /home/hexo/.ssh
    cp /root/.ssh/id_rsa.pub /home/hexo/.ssh/authorized_keys
将hexo用户添加到sudo的列表以使用sudo开启nginx服务：
```
# chmod u+w /etc/sudoers
# vi /etc/sudoers
```
在root一行下面添加：
```sh
##      user    MACHINE=COMMANDS
##
## The COMMANDS section may have other options added to it.
##
## Allow root to run any commands anywhere

root    ALL=(ALL)       ALL
hexo    ALL=(ALL)       ALL

```
最后改回权限：

    # chmod u-w /etc/sudoers

***注：此步骤也可以不做，直接用原来的用户***
### 安装nginx ###
#### ubuntu请看这里 ####
Linux (Ubuntu, Debian):  

    sudo apt-get install nginx
#### Centos 6 请看这里 ####
Linux (Fedora, Red Hat, CentOS): 

    sudo yum install nginx
如果显示没有找到nginx，那么需要进行以下操作添加：

    # vi /etc/yum.repos.d/nginx.repo
然后将下面的内容复制进去：
````sh
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1
````

### nginx 配置 ###

以下内容添加到/etc/nginx/conf.d/hexo.conf中
```sh
server {
        listen 80;
        server_name yourname;
        location / {
                root /home/hexo/www;
                index index.html index.htm;
}
}

```
修改/etc/nginx/nginx.conf 中的用户名为hexo
启动nginx服务  

```bash
su hexo  
mkdir ~/www
sudo nginx -t
sudo service nginx restart
sudo nginx -s reload

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
如果输出和上面的一致就说明配置文件没有问题，接下来就启动nginx服务，并设置为开机启动。
```shell
$ sudo systemctl start nginx
$ sudo systemctl enable nginx  
```
### 配置git ###
参照第一章的内容在服务器上安装git
安装完成后，初始化git仓库
```
$ mkdir /home/hexo/hexo && cd /home/hexo/hexo
$ git init --bare
```
将以下内容添加到hooks/post-receive中，并将post-receive文件权限改为755。
```bash
#!/bin/bash
GIT_REPO=/home/hexo/hexo
TMP_GIT_CLONE=/tmp/HexoBlog
PUBLIC_WWW=/home/hexo/www
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}	
```

    $ chmod 755 hooks/post-receive

以上就将服务器配置完毕，下面就需要配置客户端。
### 客户端 ###
在hexo根目录_config.yml中添加部署配置  
```
deploy:
- type: git
  repo: ssh://hexo@VPS的IP:ssh的端口/home/hexo/hexo
  branch: master
```
## 利用github-webhook自动部署
### 前期准备
参考[在vps上部署hexo](#在vps上部署hexo)章节安装配置：
- [创建用户](#创建hexo用户)
- [nginx](#安装nginx)
### 配置Github Webhook
首先将博客的内容(`${HEXO_dir}/source/_posts/`)托管到github上，并创建一个webhook：
![]()
在vps上搭建一个wenhook server用来监听gihub的请求，以下是基于flask的实现：
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import json
import os
import traceback

from flask import Flask, request

app = Flask(__name__)

@app.route('/webhook', methods=['POST'])
def update():
    if request.method == 'POST':
        try:
            print request.headers
            print request.json
            print os.popen("sh ~/webhook.sh").read()
        except:
            print traceback.format_exc()

    return json.dumps({"msg": "error method"})

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=8888, debug=False)
```
基本原理是当服务器收到请求后会执行一个更新脚本`webhook.sh`，其具体内容如下：
```
#!/usr/bin/env bash

cd ~/hexo_blog_static
git pull https://github.com/zhai3516/hexo_blog_static.git
rm -rf ~/hexo_blog/source/_posts/*
cp -r ~/hexo_blog_static/blog-md/* ~/hexo_blog/source/_posts/

cd ~/hexo_blog
hexo clean
hexo generate
rm -rf /var/www/hexo/*
cp -r ~/hexo_blog/public/* /var/www/hexo/
```
## 第三方服务 ##
### 验证百度、google搜索 ###
打开Hexo博客站点配置文件，添加以下两行  

    google_site_verification: *******  
    baidu-site-verification: *******  

其中*为百度站长工具和Google search console提供的html标签中的content中的内容  
如果百度的验证失败则可以下载html验证文件到主题的source文件夹下。  
### 添加站点地图 ###
先要安装插件  

    npm install hexo-generator-sitemap --save  
    npm install hexo-generator-baidu-sitemap --save

在博客的站点配置文件_config.yml中添加以下代码  

    sitemap:
    path: sitemap.xml
    baidusitemap:
    path: baidusitemap.xml

其实不添加也是可以的（网上都这么说，所以我也没加）  
### 百度自动推送 ###
将主题配置文件中的baidu_push设置为true，然后将/next/layout/_scripts文件夹下面的baidu-push.swig文件添加  
```html
{% if theme.baidu_push %}
<script>
(function(){
    var bp = document.createElement('script');
    var curProtocol = window.location.protocol.split(':')[0];
    if (curProtocol === 'https') {
        bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';        
    }
    else {
        bp.src = 'http://push.zhanzhang.baidu.com/push.js';
    }
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(bp, s);
})();
</script>
{% endif %}
```
### Local Search
- 安装 hexo-generator-searchdb，在站点的根目录下执行以下命令：  

      $ npm install hexo-generator-searchdb --save 
- 编辑 站点配置文件，新增以下内容到任意位置：
  ```
  search:
    path: search.xml
    field: post
    format: html
    limit: 10000
  ```
- 编辑 主题配置文件，启用本地搜索功能：
  ```
  # Local search
  local_search:
    enable: true
  ```

### 引用本地图片
- 安装插件  

      npm install hexo-asset-image --save
- 然后就可以使用常规方法插入图片了

### 插入数学公式
- hexo 插入数学公式着实让我头痛了很久，只不过一直需求不大所以也没怎么搞，直到写{% post_link SDNE笔记 SDNE笔记 %}这篇博客时没办法才开始解决。其实主要的问题就是多行公式的使用，目前还是没有什么好的办法，只好安装插件解决，但是要加math tag还是比较烦的。具体步骤如下：
- 在hexo根目录下执行：

      npm install hexo-math --save
- 修改_config.yml 文件，加入：
  ```sh
  math:
    engine: 'mathjax' # or 'katex'
    mathjax:
      src: custom_mathjax_source
      config:
        # MathJax config
    katex:
      css: custom_css_source
      js: custom_js_source # not used
      config:
        # KaTeX config
  ```
- 在 NexT 主题的配置文件(Hexo blog 根目录下的 themes\next\ _config.yml 文件)使用以下设置:
    ```sh
    mathjax:
      enable: true
      per_page: false
      cdn: //cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML
   
    ```
- 使用:
    ```
    {% math %}
    \begin{align*} 
    x &= a +b+c+d\\
    &= e +f\\
    &= g
    \end{align*}
    {% endmath %}
    ```
- 效果：  

{% math %}
\begin{align*} 
x &= a +b+c+d\\
&= e +f\\
&= g
\end{align*}
{% endmath %}