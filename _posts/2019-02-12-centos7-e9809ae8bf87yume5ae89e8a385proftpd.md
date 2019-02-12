---
ID: 2408
post_title: centos7 通过yum安装proftpd
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2019/02/12/centos7-%e9%80%9a%e8%bf%87yum%e5%ae%89%e8%a3%85proftpd/'
published: true
post_date: 2019-02-12 18:39:29
---
<h1 class="title">centos7 通过yum安装proftpd</h1>
&nbsp;
<pre>#1.安装epel源
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

#2.安装proftpd
yum install -y proftpd openssl proftpd-utils

#3.启动proftpd
systemctl start proftpd.service
systemctl enable proftpd.service

#4.创建ftp登录用户
#a.创建ftp组：
groupadd ftpgroup

#b.创建ftp用户，并关联ftp目录：
   useradd -G ftpgroup ftptest -s /sbin/nologin -d /home/ftptest/

#c.设置ftptest用户密码：
passwd ftptest

#5.给目录设置权限，
chmod -R 777 /home/ftptest/

#6.完毕，使用ftp客户端测试proftpd是否正常上传下载文件。</pre>