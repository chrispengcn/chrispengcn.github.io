---
ID: 247
post_title: Centos 设置网卡IP地址方法
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/08/31/centos-%e8%ae%be%e7%bd%ae%e7%bd%91%e5%8d%a1ip%e5%9c%b0%e5%9d%80%e6%96%b9%e6%b3%95/'
published: true
post_date: 2013-08-31 20:26:06
---
<p>１.修改网卡配置　编辑：vi /etc/sysconfig/network-scripts/ifcfg-eth0 <p><img alt="" src="http://pic002.cnblogs.com/images/2011/359567/2011121213042997.png"> <p>　DEVICE=eth0 #描述网卡对应的设备别名，例如ifcfg-eth0的文件中它为eth0<br>　BOOTPROTO=static #设置网卡获得ip地址的方式，可能的选项为static，dhcp或bootp，分别对应静态指定的 ip地址，通过dhcp协议获得的ip地址，通过bootp协议获得的ip地址<br>　BROADCAST=192.168.0.255 #对应的子网广播地址<br>　HWADDR=00:07:E9:05:E8:B4 #对应的网卡物理地址 <p>　IPADDR=12.168.0.33 #如果设置网卡获得 ip地址的方式为静态指定，此字段就指定了网卡对应的ip地址<br>　NETMASK=255.255.255.0 #网卡对应的网络掩码<br>　NETWORK=192.168.0.0 #网卡对应的网络地址 <p>2.修改网关配置 <p>　编辑：vi /etc/sysconfig/network　修改后如下：　 <p><img alt="" src="http://pic002.cnblogs.com/images/2011/359567/2011121213122055.png"> <p>　NETWORKING=yes(表示系统是否使用网络，一般设置为yes。如果设为no，则不能使用网络，而且很多系统服务程序将无法启动)<br>　HOSTNAME=centos(设置本机的主机名，这里设置的主机名要和/etc/hosts中设置的主机名对应)<br>　GATEWAY=192.168.0.1(设置本机连接的网关的IP地址。) <p><strong>我在修改这里打开编辑时前三项已经默认有了所以只增加了GATEWAY</strong> <p>3.修改DNS 配置 <p>&nbsp; 编辑：vi /etc/resolv.conf　修改后如下：　 <p><img alt="" src="http://pic002.cnblogs.com/images/2011/359567/2011121213190377.png"> <p>&nbsp; nameserver　即是DNS服务器ＩＰ地址，第一个是首选，第二个是备用。 <p>4.重启网络服务 <p>　　执行命令： <p>　　service network restart 　或&nbsp; /etc/init.d/network restart