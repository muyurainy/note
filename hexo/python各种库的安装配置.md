---
title: python各种库的安装配置
date: 2017-03-29 17:41:53
tags: [python, Machine Learning]
category: 
- Machine Learning
- Python
toc: true
---
### 安装networkx ###  
安装networkx之前要安装画图工具matplotlib，以及矩阵运算工具numpy,于是我们执行:  
```
pip install numpy
pip install matplotlib
pip install networkx
```
### 安装bs4、lxml ###  
安装bs4  
```
pip install
```

安装lxml  
```
pip install wheel
pip install ....wheel
```

### 安装配置jupyter
安装：
```bash
pip install jupyterlab
```

配置:
- 执行命令生成配置文件：
    ```bash
    jupyter lab --generate-config
    ```
- 生成秘钥，在`ipython`中执行：
    ```python
    from notebook.auth import passwd
    passwd()
    ```
    输入两次密码并复制得到的hash码。
- 在配置文件里添加：
    ```python
    c.NotebookApp.ip='0.0.0.0'
    c.NotebookApp.open_browser=False # 关闭自动打开浏览器
    c.NotebookApp.port=8888 # 指定端口
    c.NotebookApp.password =u'sha1:addsadsa' # 复制前一步生成的hash密钥
    ```
- 使用nohup提交到后台运行：
    ```bash
    nohup jupyter lab >> nohup.out 2>&1 &
    ```
- 如果遇到错误`OSError: [Errno 13] Permission denied: '/run/user/1004/jupyter'`，执行一下命令：
    ```bash
    echo 'unset XDG_RUNTIME_DIR' >> ~/.bashrc
    source ~/.bashrc
    ```