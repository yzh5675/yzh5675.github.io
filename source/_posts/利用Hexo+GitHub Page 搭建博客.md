---
title: 利用Hexo+GitHub Page 搭建博客
date: 2021-09-08 09:38:35
author: 别听学唱情歌
img: https://yzh5675.oss-cn-beijing.aliyuncs.com/blog/img/akl03
categories: Hexo
abbrlink: 3eeb
top: false
cover: true
toc: false
mathjax: false
tags:
- Hexo
- GitHub
---
### 搭建博客

#### 前言

 收到很多留言和联系我怎么搭建一个博客，但是由于学习比较忙，所有就没有给大家都回复到，还有很多同学想让我给搭建博客，但是我觉得授人以鱼不如授人以渔，所以整理出来一些我搭建过程中的主要步骤。在之后如果有时间或者有人想要看我是如何改的博客样式的，可以留言和评论，我再给出教程，在按照步骤搭建的过程中遇到问题也可以联系我。

#### 1.安装工具

在搭建博客的前期需要安装相关的工具分别是node和git，这两个工具都可以在官网上面下载，安	装和配置可以在网上自行搜索。



#### 2.注册github账号

因为我们要搭建的是一个静态博客，所有要把文件都放在GitHub的仓库里，所以需要注册一个	GitHub账号以及新建一个GitHub仓库，这个比较简单可以自行搜索。

- 这里有一个要注意的环节就是在建立仓库的时候仓库名必须是`用户名.github.io`
- 在仓库有一个index.html文件时，在浏览器中输入`https://用户名.github.io`就可以看到index.html里的内容，github pages就创建好了（这一步只是试验，不是必须操作）

#### 3. 配置git用户信息

​	打开git输入

```
git config --global user.name "你的github用户名"
git config --global user.email "你的github邮箱"
```

#### 4.安装hexo静态博客框架发布到github pages

打开git输入

```
npm install -g hexo-cli
hexo init 文件夹名
cd 文件夹名
npm install 
```

后续操作都在上面新建的文件夹里操作

hexo框架的本地搭建完成之后就输入命令查看效果（不是必要操作）

```
   hexo g
   hexo s
```

#### 5.将本地博客发布到github pages中

下载安装发布的插件

```
npm install hexo-deployer-git --save
```

将本地目录和github关联起来

生成秘钥和公钥

```
ssh-keygen -t rsa -C "你的github邮件地址"
```

- 一直回车，在提示目录下找到.ssh文件夹， 把公钥也就是id_rsa.pub中的内容复制到github的setting里的SSH and GPG keys中添加

测试链接是否成功

```
ssh -T git@github.com
```

#### 6.配置站点目录并发布

用编辑器打开_config.yml文件，对博客进行个性化配置

```
title: 你的博客名 
subtitle: 博客的副标题，有些主题支持 
description: 博客描述
keywords: 博客关键词 
author: 作者，在文章中显示 
language: 博客语言语种 
timezone: 时区
```


在文件的最低部添加repo项和branch项，并添加如下信息

```
type: git 
repo: git@github.com:Github用户名/github用户名.github.io.git
//也可使用https地址，如：https://github.com/Github用户名/Github用户名.github.io.git branch: master
```

最后就是生成页面并且发布在github pages上，（之后写文章放在/source/_posts/目录下也上下面这三条命令上传)

```
hexo clean    //清缓存
hexo g        //渲染
hexo d        //上传
hexo clean && hexo g -d		//这是三条一起运行的命令
```

这时候打开`https://用户名.github.io`你就可以看到你的博客已经更新了。