---
title: 本地搭建showdoc文档管理工具
copyright: true
date: 2019-08-30 14:33:05
categories:
tags:
top:
---



## 安装php？

> 又开始踩坑了
>
> 本来想通过docker方式安装，无奈运行命令的时候发现，只适用于Linux环境。。浪费2小时+

<!-- more -->

1. 直接下载并安装[phpstudy](http://phpstudy.php.cn/)

2. 看看php版本

   {% asset_img 20190830171018.png php-version image %}

    这打开方式不对啊。。啥情况？

   再看看phpstudy的界面里面，php运行环境都没装？

   {% asset_img 20190830171204.png no-php image %}

   此时的小c一脸黑人问号。。。看到了这里

   {% asset_img 20190830171421.png php-website image %}

   打开phpstudy里面的apache

   战战兢兢的在浏览器地址栏敲下了localhost并回车：

   {% asset_img 20190830171559.png php-success image %}

   所以，安装成功了呗？

> php是世界上最好（安装）的语言 ！！！



## 下载&部署代码

地址：[https://github.com/star7th/showdoc](https://github.com/star7th/showdoc)

git clone了半天也不动，直接下zip包了。。。

下完以后复制到上述网站截图的，物理路径WWW文件夹下面

localhost/showdoc 就完事了



## 项目&成员&文档管理

这好像不是本篇博客的内容。。。

其实这个工具也比较傻瓜式，很简单，随便点点就会了~就不写了吧。。



## 为什么不需要安装php依赖？

