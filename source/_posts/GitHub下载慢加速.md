---
title: GitHub下载慢加速
date: 2021-09-13 00:38:35
author: 别听学唱情歌
img: https://yzh5675.oss-cn-beijing.aliyuncs.com/blog/img/akl02
top: false
cover: true
categories: GitHub
toc: false
mathjax: false
abbrlink: '0'
tags:
- GitHub
---

## Github 下载慢加速

解决GitHub 下载 release 资源太慢的办法

### 1. 使用下载器下载

  复制 release 资源链接，使用 `迅雷` 或者 `Free Download Manager` 直接下载 http://github.com 开头的原链接。
  使用迅雷下载不是很稳定，有时候速度可以秒下，有时候下载不下来。可以多试几次。 FDM暂时还没测试过。

优点： 有资源时可以下载快，从官方原链接下载，安全性可靠性相对较好

不足： 依赖下载器，需要看资源情况

### 2. 使用代下载服务l

  复制 release 资源链接，使用github 代下载 ：

​	http://gitd.cc/ 
​	http://github.b15.me/ 
优点： 不需要下载器，随时都可以打开网站使用

不足： 网站可用性、安全性可靠性取决于代下载服务，文件会被二次压缩，且下载文件名会改变

### 3. Github下载加速插件

  安装gitHub加速插件：

https://fhefh2015.github.io/Fast-GitHub/

### 4. 使用GitHub 反代域名替换

  使用GitHub 反代域名替换

https://fastgit.org/
https://www.gitclone.com
https://cdn.con.sh
http://gitcdn.link/

## Github 访问慢加速

内容整理来自网络。

### 1、获取相关域名

获取Github相关网站的域名。

#### 方式一：

访问 https://github.com

F12 查看网络，查看域名。

#### 方式二：

直接访问：https://github.com.ipaddress.com

#### 2、获取相关域名对应IP

访问 https://www.ipaddress.com

分别输入

github.global.ssl.fastly.net
github.com

查询ip地址

### 3、配置hosts

windows 系统的 hosts 文件的位置如下：C:\Windows\System32\drivers\etc\hosts ;

mac/linux 系统的 hosts 文件的位置如下：/etc/hosts 。

下面是我的配置，更新日期 2020-09-20

```
140.82.112.3 github.com
199.232.69.194 github.global.ssl.fastly.net

199.232.68.133  githubusercontent.com
185.199.108.153 assets-cdn.github.com
185.199.109.153 assets-cdn.github.com
185.199.110.153 assets-cdn.github.com
185.199.111.153 assets-cdn.github.com
```

### 4、刷新DNS

```bash
ipconfig /flushdns
```

应该快点了吧？

隔一段时间查一次，IP 不会一直不变，不定期更新。

### 5、DNS命令解析IP

DNS查看

```BASH
ipconfig /all
```

底下可以看到 DNS 服务器IP相关信息。

指定DNS 解析域名：

```bash
nslookup github.com  8.8.8.8
```

能显示出DNS服务器IP，说明github.com 是可以访问的，可以尝试修改自己电脑DNS再试试。

```
谷歌：8.8.8.8
阿里：223.5.5.5 或 223.6.6.6
114 : 114.114.114.114
百度:180.76.76.76
360 : 101.226.4.5
南方电信: 180.153.225.136
北京联通 202.106.0.20 202.106.196.115
```

执行命令：

```bash
ipconfig /flushdns
```

