---
ID: 249
post_title: '-bash: wget: command not found的两种解决方法'
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/08/31/bash-wget-command-not-found%e7%9a%84%e4%b8%a4%e7%a7%8d%e8%a7%a3%e5%86%b3%e6%96%b9%e6%b3%95/'
published: true
post_date: 2013-08-31 21:02:43
---
<p>今天给服务器安装新LNMP环境时，wget 时提示 -bash:wget command not found,很明显没有安装wget软件包。一般linux最小化安装时，wget不会默认被安装。<br>可以通过以下两种方法来安装：<br>1、rpm 安装<br>rpm 下载源地址：http://mirrors.163.com/centos/6.2/os/x86_64/Packages/<br>下载wget的RPM包：http://mirrors.163.com/centos/6.2/os/x86_64/Packages/wget-1.12-1.4.el6.x86_64.rpm<br>rpm ivh wget-1.12-1.4.el6.x86_64.rpm 安装即可。<br>如果客户端用的是SecureCRT,linux下没装rzsz 包时，rz无法上传文件怎么办？我想到的是安装另一个SSH客户端：SSH Secure Shell。然后传到服务器上安装，这个比较费劲，所以推荐用第二种方法，不过如果yum包也没有安装的话，那就只能用这种方法了。<br>2、yum安装<br>yum -y install wget<br>显然第二种方法比较简单快捷。</p>