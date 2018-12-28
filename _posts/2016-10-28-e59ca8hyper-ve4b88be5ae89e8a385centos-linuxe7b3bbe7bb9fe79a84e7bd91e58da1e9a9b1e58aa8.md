---
ID: 553
post_title: >
  在Hyper-V下安装CentOS
  Linux系统的网卡驱动
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2016/10/28/%e5%9c%a8hyper-v%e4%b8%8b%e5%ae%89%e8%a3%85centos-linux%e7%b3%bb%e7%bb%9f%e7%9a%84%e7%bd%91%e5%8d%a1%e9%a9%b1%e5%8a%a8/'
published: true
post_date: 2016-10-28 22:46:16
---
<h3>在Hyper-V下安装CentOS Linux系统的网卡驱动 </h3> <p>解决办法是安装微软提供的：Linux Integration Services Version v3.2 for Hyper-V工具(下载地址)：下载下来之后是一个iso文档，挂着在linux的虚拟光驱下执行安装命令： <p>1. 下载微软虚拟机Linux集成服务包 Linux Integration Services v3.2 <p>http://www.microsoft.com/zh-cn/download/details.aspx?id=28188 <p>2. 加载光盘镜像 <p>【媒体】-【DVD驱动器】-【插入磁盘】选择刚下载的iso文件，加载光盘镜像 <p><a href="https://c.ikafan.com/upload/b4/20/b42021b16bc5aeda54203ccfd7071a85.jpg"><img title="在Hyper-V下安装CentOS Linux系统的网卡驱动" alt='在Hyper-V下安装CentOS Linux系统的网卡驱动' src="http://hss5.com/wp-content/uploads/2016/10/b42021b16bc5aeda54203ccfd7071a85_thumb.jpg"></a><br>3. 挂载光盘镜像并安装 <p># mount /dev/cdrom /media <p># cd /media <p># ./install.sh <p>4. 网卡、DNS相关配置 <p># vi /etc/sysconfig/network <p>内容如下： <p>NETWORKING=yes <p>NETWORKING_IPV6=no <p>HOSTNAME=Cnyunwei.com <p># vi /etc/sysconfig/network-scripts/ifcfg-eth0 <p>内容如下： <p>DEVICE=eth0 <p>ONBOOT=yes <p>IPADDR=192.168.0.88 <p>NETMASK=255.255.255.0 <p>GATEWAY=192.168.0.1 <p># vi /etc/resolv.conf <p>内容如下： <p>nameserver 8.8.8.8 <p>5、重启系统启用网卡 <p># reboot <p>重启系统后通过ifconfig就可以查看到刚配置的ip了