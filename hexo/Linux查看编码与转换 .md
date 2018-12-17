---
title: Linux查看编码与转换 
date: 2017-04-06 17:41:53
tags: Linux使用
category: 
-  效率
toc: true
---

> 最近在做NLP的时候遇到一个问题就是讲Windows的文件拷到Linux编码报错了，折腾了半天终于解决。

* 查看文件编码：  

        $file filename  
        $file news_tensite_xml.smarty.dat   
        news_tensite_xml.smarty.dat: Non-ISO extended-ASCII text, with very long lines  

嗯，查看编码后发现不是utf-8，查阅资料说可以用vim来修改编码，但由于我的文件比较大，所以用vim打开不现实，故采用其他方法。

* 尝试使用iconv  

查阅资料后发现有个东西叫iconv，所以执行：  

    iconv -t UTF-8 filename >log.txt
    iconv: illegal input sequence at position 48
有朋友说可以遍历iconv支持的所有编码方式然后选出来可以编码的方式，感觉有点麻烦，故弃之。。。  

* 使用enca  
使用apt-get 安装enca  
然后查看文件类型：  

    $ enca -L zh_CN test.log  
    Simplified Chinese National Standard; GB2312  

感觉这个编码方式比较科学，然后发现这货还可以编码转换（happy～），于是乎执行：  

    $ enca -L zh_CN -x UTF-8 < news_tensite_xml.smarty.dat > sougou.smart.xml
成功！

