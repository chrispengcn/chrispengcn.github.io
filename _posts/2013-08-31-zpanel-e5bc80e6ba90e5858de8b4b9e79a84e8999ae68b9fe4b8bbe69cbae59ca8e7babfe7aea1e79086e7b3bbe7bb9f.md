---
ID: 243
post_title: >
  ZPanel-开源免费的虚拟主机在线管理系统
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/08/31/zpanel-%e5%bc%80%e6%ba%90%e5%85%8d%e8%b4%b9%e7%9a%84%e8%99%9a%e6%8b%9f%e4%b8%bb%e6%9c%ba%e5%9c%a8%e7%ba%bf%e7%ae%a1%e7%90%86%e7%b3%bb%e7%bb%9f/'
published: true
post_date: 2013-08-31 19:45:11
---
<p>建站的都知道cPanel、DirectAdmin等强大的主机控制面板，但他们都是收费的，而且收取的费用还不低。免费的Kloxo的大家可能也比较熟悉的。今天给大家介绍的是ZPanel，这是个功能不错，且开源免费的虚拟主机在线管理系统。 <p>现在的VPS价格不高，很多朋友都开始玩VPS了。买了VPS后最重要的就是装WEB环境了，尽管LNMP一键安装包已经很是强大，但还是不是人喜欢有控制面板的，这样方便管理。国人也有开发相关的软件，不过今天的重点是ZPanel。 <p>ZPanel是典型的免费虚拟主机控制面板，支持安装在Windows，Linux，UNIX和MacOSX等多个操作系统上。其控制面板简洁易 用，用户可以创建和管理MySQL数据库，电子邮件信箱等，也可以在一台服务器绑定多个域名。且ZPanel能与 MySQL，hMailServer（Windows平台），Postfix（Linux平台等），Filezilla（Windows平台）或 ProFTPd（Linux平台等）等多个软件完美兼容，默认情况下安装了Apache, PHP, Postfix等软件。 <p>下面来看看ZPanel的安装吧。目前，ZPanel最新版本是6.1.1. <p>在Ubuntu Linux上安装ZPanel<br>wget http://sourceforge.net/projects/zpanelcp/files/releases/6.1.1/zpanel-6.1.1.tar.gz<br>mkdir /etc/zpanel<br>tar -zxvf zpanel-6.1.1.tar.gz -C /etc/zpanel/<br>chmod +x /etc/zpanel/lib/dev/zpinstall_ubuntu.sh<br>/etc/zpanel/lib/dev/zpinstall_ubuntu.sh <p>在CentOS Linux上安装ZPanel <p>wget http://sourceforge.net/projects/zpanelcp/files/releases/6.1.1/zpanel-6.1.1.tar.gz<br>mkdir /etc/zpanel<br>tar -zxvf zpanel-6.1.1.tar.gz -C /etc/zpanel/<br>chmod +x /etc/zpanel/lib/dev/zpinstall_centos.sh<br>/etc/zpanel/lib/dev/zpinstall_centos.sh <p>WIN主机商的教材去ZPanel官网看吧。 <p>ZPanel官方网址：http://www.zpanelcp.com/ <p>安装好后，默认没有文件管理器，只能在后台添加FTP帐号，然后再通过FTP软件登录管理文件，有些不方便，既然用了控制面板，何不把把文件管理器也添加进去呢。 <p>cd /etc/zpanel<br>wget http://downloads.sourceforge.net/project/zpanelcp/modules/zp5/advanced_file_manager/Advanced_Manager.zip<br>unzip Advanced_Manager.zip<br>mv file apps<br>mv afilemanager modules/storage<br>vi modules/storage/afilemanager/index.php <p>第六行改成 <p>$url = “/apps/file/user.php?word=/var/zpanel/hostdata/”; <p>上面只是ZPanel的一个插件，它还有其他实用的插件及中文翻译，当然你如果不喜欢Apache也可以自己配置Nginx替换，这些都去其官方维基及论坛学习吧。 <p>项目地址：<a href="http://zpanelcp.sourceforge.net/">http://zpanelcp.sourceforge.net/</a></p>