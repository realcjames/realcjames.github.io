---
title: Hexo搭建教程、配置和踩坑记录
copyright: true
date: 2019-08-30 15:07:41
categories:
tags:
top:
---



> 其实这才应该是除了hello world以外的第一篇文章啊喂，可是，我已经搭好了这个博客，为什么还需要写一篇教程呢？只有可能是为了吹B吧。。。



## 搭建&初步配置

<!-- more -->

不好意思，已经过去快半年了。。。我记不清了。。。

> 吹不起来了。。。。



## 踩坑记录

#### 添加图片

天真的小c本以为：

> 1. 在source下面新建assets/images目录
> 2. 把png放到目录下面
> 3. 文章里面直接相对地址../assets/images/xxx.png

结果部署上去以后就

{% asset_img 20190830151348.png no-image image %}

还是去看看[文档吧](https://hexo.io/zh-cn/docs/asset-folders)

> 1. config.yml里面的post_asset_folder设置为true，这样每次hexo new都会在同目录下新建一个跟文章同名的文件夹用来存放资源文件
> 2. 文章里面通过相对路径去引用{% asset_img example.jpg This is an example image %}

来看看实际生成的标签效果：

{% asset_img 20190830151912.png html-image image %}

> 这样才对了。。。毕竟hexo是编译成html以后再部署上去的。。路径肯定不是相对引用这么简单