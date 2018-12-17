---
title: Structural Deep Network Embedding 笔记
date: 2018-01-04 20:32:00
tags: [Deep learning, Network Embedding]
category: 
-  Deep Learning
-  Network Embedding
toc: true
mathjax: true
---
# 前言

# 背景知识
[论文链接](http://211.81.63.130/cache/9/03/www.kdd.org/f7ed7ee5c8adb5e586160f0375f71dd8/rfp0191-wangAemb.pdf)
## Laplacian Eigenmaps
参考[博客](http://blog.csdn.net/qrlhl/article/details/78066994)  
### 推导

{% math %}
\begin{gathered}
\sum_{i=1}^{n}\sum_{j=1}^{n}||y_i-y_j||^2W_{ij}
\\=\sum_{i=1}^{n}\sum_{j=1}^{n}(y_i^Ty_i-2y_i^Ty_j+y_j^Ty_j)W_{ij}
\\= \sum_{i=1}^{n}(\sum_{j=1}^nW_{ij})y_i^Ty_i+\sum_{j=1}^{n}(\sum_{i=1}^nW_{ij})y_j^Ty_j-2\sum_{i=1}^{n}\sum_{j=1}^{n}y_i^Ty_jW_{ij}
\\=2\sum_{i=1}^{n}D_{ii}y_i^Ty_i-2\sum_{i=1}^{n}\sum_{j=1}^{n}y_i^Ty_jW_{ij}
\\=2\sum_{i=1}^{n}(\sqrt{D_{ii}}y_i)^T(\sqrt{D_{ii}}y_i)-2\sum_{i=1}^ny_i^T(\sum_{j=1}^ny_jW{ij})
\\=2trace(Y^TDY) - 2\sum_{i=1}^ny_i^T(YW)_i
\\=2trace(Y^TDY)-2trace(Y^TWY)
\\=2trace[Y^T(D-W)Y]
\\=2trace(Y^TLY)
\end{gathered}
{% endmath %}
## 深度置信网络 (Deep Belief Network, DBN)
### 受限玻尔兹曼机 (Restricted Boltzmann Machines, RBM)
几篇参考资料：
- [https://www.cnblogs.com/jhding/p/5687696.html](https://www.cnblogs.com/jhding/p/5687696.html)
- [https://github.com/muyurainy/DeepLearningTutorial/blob/master/7_Restricted_Boltzmann_Machine_%E5%8F%97%E9%99%90%E6%B3%A2%E5%B0%94%E5%85%B9%E6%9B%BC%E6%9C%BA.md](https://github.com/muyurainy/DeepLearningTutorial/blob/master/7_Restricted_Boltzmann_Machine_%E5%8F%97%E9%99%90%E6%B3%A2%E5%B0%94%E5%85%B9%E6%9B%BC%E6%9C%BA.md)
- [http://blog.csdn.net/itplus/article/details/19168937](http://blog.csdn.net/itplus/article/details/19168937)