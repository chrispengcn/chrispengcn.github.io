---
ID: 182
post_title: >
  CentOS 5
  安装免费虚拟主机控制面板ZPanel
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/08/31/centos-5-%e5%ae%89%e8%a3%85%e5%85%8d%e8%b4%b9%e8%99%9a%e6%8b%9f%e4%b8%bb%e6%9c%ba%e6%8e%a7%e5%88%b6%e9%9d%a2%e6%9d%bfzpanel/'
published: true
post_date: 2013-08-31 02:04:58
---
<h3>作者:<a href="http://www.centos.bz/author/admin/">朱 茂海</a> /分类:<a href="http://www.centos.bz/category/control-panels/">控制面板</a> /Tag:<a href="http://www.centos.bz/tag/zpanel/">ZPanel</a></h3> <p><a href="http://www.centos.bz/tag/zpanel/">ZPanel</a>是一个免费的虚拟主机控制面板,支持<a href="http://www.centos.bz/category/other-system/windows/">Windows</a>,<a href="http://www.centos.bz/tag/linux/">Linux</a>,UNIX和MacOSX等多个操作系统的服务器。ZPanel能使家用或专业服务器成为一个完全成熟，易于使用和管理的网站托管服务器。<br>对于易于使用的控制面板，用户可以创建和管理<a href="http://www.centos.bz/category/mysql/">MySQL</a>数据库，电子邮件信箱，代理和分销的名单，也能在一台服务器设置托管多个域名。<br>这个软件使用了其它免费或开源的软件提供免费许可证，成本效益解决方案。ZPanel能与MySQL,hMailServer(Windows平台)，<a href="http://www.centos.bz/category/email/postfix/">Postfix</a>(Linux平台等)，Filezilla(Windows平台)或<a href="http://www.centos.bz/category/ftp/proftpd/">ProFTPd</a>(Linux平台等)等多个软件完美兼容。<br>下面我们来介绍如何安装在<a href="http://www.centos.bz/">CentOS</a> 5.5系统上。 <ol> <li>wget http://sourceforge.net/projects/zpanelcp/files/releases/6.1.1/zpanel-6.1.1.tar.gz  <li>mkdir /etc/zpanel  <li>tar -zxvf zpanel-6.1.1.tar.gz -C /etc/zpanel/  <li>chmod +x /etc/zpanel/lib/dev/zpinstall_centos.sh  <li>/etc/zpanel/lib/dev/zpinstall_centos.sh</li></ol> <p><a href="http://www.centos.bz/wp-content/uploads/2011/06/ZPanel1.jpeg"><img title="ZPanel1" alt="" src="http://www.centos.bz/wp-content/uploads/2011/06/ZPanel1.jpeg" width="700"></a><br>官方网站：<a href="http://www.zpanelcp.com/">http://www.zpanelcp.com/</a>