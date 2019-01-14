---
ID: 562
post_title: >
  Ubuntu下配置samba实现文件夹共享
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2016/11/28/ubuntu%e4%b8%8b%e9%85%8d%e7%bd%aesamba%e5%ae%9e%e7%8e%b0%e6%96%87%e4%bb%b6%e5%a4%b9%e5%85%b1%e4%ba%ab/'
published: true
post_date: 2016-11-28 15:02:24
---
<p>&nbsp; <h4><a href="http://www.cnblogs.com/phinecos/archive/2009/06/06/1497717.html">Ubuntu下配置samba实现文件夹共享</a></h4> <p>一. samba的安装: <p>sudo apt-get insall samba<br>sudo apt-get install smbfs <p>二. 创建共享目录: <p>mkdir /home/phinecos/share<br>sodu chmod 777 /home/phinecos/share <p>三. 创建Samba配置文件: <p>1. 保存现有的配置文件 <p>sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak <p>2. 修改现配置文件 <p>sudo gedit /etc/samba/smb.conf <p>在smb.conf最后添加 <p>[share]<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path = /home/phinecos/share<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; available = yes<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; browsealbe = yes<br>public = yes<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; writable = yes <p>四. 创建samba帐户 <p>&nbsp; sudo touch /etc/samba/smbpasswd<br>&nbsp; sudo smbpasswd -a phinecos <p>然后会要求你输入samba帐户的密码 <p>［如果没有第四步，当你登录时会提示 session setup failed: NT_STATUS_LOGON_FAILURE］ <p>五. 重启samba服务器 <p>sudo /etc/init.d/samba restart <p>六. 测试 <p>smbclient -L //localhost/share <p>七，使用 <p>可以到windows下输入ip使用了，在文件夹处输入 "\" + "Ubuntu机器的ip或主机名" + "\" + "share"</p>