---
ID: 1640
post_title: linux下mysql数据库导入导出命令
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/05/linux%e4%b8%8bmysql%e6%95%b0%e6%8d%ae%e5%ba%93%e5%af%bc%e5%85%a5%e5%af%bc%e5%87%ba%e5%91%bd%e4%bb%a4/'
published: true
post_date: 2018-12-05 23:01:57
---
首先linux 下查看mysql相关目录
root@ubuntu14:~# whereis mysql
mysql:
/usr/bin/mysql----   mysql的运行路径
/etc/mysql
/usr/lib/mysql-----   mysql的安装路径
/usr/bin/X11/mysql
/usr/share/mysql
/usr/share/man/man1/mysql.1.gz
此外还有一个：
var/lib/mysql --------mysql数据库data文件的存放路径

确定了运行路径，执行导入、导出mysql数据库命令
一、导出数据库用mysqldump命令
（注意:先cd到mysql的运行路径下，再执行一下命令）：
1、导出数据和表结构：
mysqldump -u用户名 -p密码 数据库名 &gt; 数据库名.sql
mysqldump -uroot -p dbname &gt; dbname .sql
敲回车后会提示输入密码
2、只导出表结构
mysqldump -u用户名 -p密码 -d 数据库名 &gt; 数据库名.sql
mysqldump -uroot -p -d dbname &gt; dbname .sql

二、导入数据库
1、首先建空数据库
mysql&gt;create database dbname ;
2、导入数据库
方法一：
（1）选择数据库
mysql&gt;use dbname ;
（2）设置数据库编码
mysql&gt;set names utf8;
（3）导入数据（注意sql文件的路径）
mysql&gt;source /home/xxxx/dbname .sql;
方法二：
mysql -u用户名 -p密码 数据库名 &lt; 数据库名.sql