---
ID: 1553
post_title: >
  CentOS Web
  Panel面板-CentOS系统最优秀的免费服务器控制面板
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/03/centos-web-panel%e9%9d%a2%e6%9d%bf-centos%e7%b3%bb%e7%bb%9f%e6%9c%80%e4%bc%98%e7%a7%80%e7%9a%84%e5%85%8d%e8%b4%b9%e6%9c%8d%e5%8a%a1%e5%99%a8%e6%8e%a7%e5%88%b6%e9%9d%a2%e6%9d%bf/'
published: true
post_date: 2018-12-03 16:26:01
---
<div class="postmeat ac">
<div>一般来说我们使用的面板基本上都是可以在Ubuntu、CentOS、Debian等Linux各个发行版本上使用，例如宝塔面板、VestaCP面板、ISPConfig面板等，都是可以在Linux上安装并运行了。不过，今天要分享的CentOS Web Panel却是“与从不同”。</div>
</div>
<div class="entry">

从名字就可以看出，CentOS Web Panel是一个专门为CentOS系统打造的VPS控制面板，功能可以说非常地强大，CWP会在您的服务器上自动安装完整的LAMP，其中包括：apache，php，phpmyadmin，webmail，mailserver 。

CentOS Web Panel自带了DNS系统、邮局系统、第三方插件、CSF防火墙、脚本安装等，CentOS Web Panel最为出众的就是在服务器管理上，例如可以调整Apache配置、切换PHP版本、服务器性能监控、安全防护、SSL证书管理、Letsencrypt启用等等。

总之，用了CentOS Web Panel（CWP）之后称之CentOS系统最优秀的免费服务器控制面板也为过，只不过由于功能太过于全面和复杂，上手CentOS Web Panel也需要不少的时间，况且CentOS Web Panel不适合小内存的VPS，资源消耗还是有点大。

<img class="alignnone size-full wp-image-1582" src="http://hss5.com/wp-content/uploads/2018/12/aa01-1.gif" width="680" height="366" alt="" />

更多的VPS控制面板可以查看我之前建立的专题页面：服务器控制面板榜单，比较适合新手朋友建站以及运用到正式的生产环境中的，可以试试：

Linux VPS建站工具LNMP 1.4安装与使用-SSL自动配置续期和多版本PHP支持

OneinStack一键安装脚本-轻松部署Let’s Encrypt证书配置Https站点

新版BT.cn宝塔VPS主机面板建站使用体验-清爽傻瓜式操作功能全面
<h2>一、CentOS Web Panel安装</h2>
CentOS Web Panel支持的操作系统有CentOS 6, RedHat 6 或者 CloudLinux 6, CentOS 7 ，32位操作系统要求最少512MB 内存，64位的操作系统要求至少1024 MB内存。

CWP 官网：

http://centos-webpanel.com/
<h4>1.1 修改主机名</h4>
CentOS6.x 修改主机名主要修改两处：一处是/etc/sysconfig/network，另一处是/etc/hosts。执行：vim /etc/sysconfig/network，将里面一行 HOSTNAME=localhost.localdomain ，修改 localhost.localdomain 为你的主机名。

然后执行：vim /etc/hosts，将一行 127.0.0.1 localhost.localdomain localhost 。其中 127.0.0.1 是本地环路地址， localhost.localdomain 是主机名(hostname)，也就是你需要修改的。localhost 是主机名的别名（alias），它会出现在Konsole的提示符下。将第二项修改为你的主机名（第三项可选）。

将上面两个文件修改完后，并不能立刻生效。如果要立刻生效的话，可以用 hostname www.wzfou.com 作临时修改，它只是临时地修改主机名，系统重启后会恢复原样的。但修改上面两个文件是永久的，重启系统会得到新的主机名。最后，重启后查看主机名 uname -n 。如下：

<img class="alignnone size-full wp-image-1583" src="http://hss5.com/wp-content/uploads/2018/12/aa02-1.gif" width="680" height="366" alt="" />
<h4>1.2 安装前准备</h4>
安装CWP依赖环境：
<ul>
 	<li>yum -y install wget</li>
</ul>
升级系统：
<ul>
 	<li>yum -y update</li>
</ul>
重启系统：
<ul>
 	<li>reboot</li>
</ul>
<h4>1.3 一键安装</h4>
CWP面板默认会安装以下组件：
<ul>
 	<li>– Apache Web服务器（Mod Security +自动更新规则可选）
– PHP 5.6（suPHP，SuExec + PHP版本切换器）
– MySQL / MariaDB + phpMyAdmin
– Postfix + Dovecot + roundcube webmail（防病毒，Spamassassin可选）
– CSF防火墙
– 文件系统锁定（没有更多的网站黑客入侵，所有的文件都被锁定）
– 备份（可选）
– 用于服务器配置的AutoFixer</li>
</ul>
CentOS 6安装:
<ul>
 	<li>cd /usr/local/src
wget http://centos-webpanel.com/cwp-latest
sh cwp-latest</li>
</ul>
CentOS 7安装：
<ul>
 	<li>cd /usr/local/src
wget http://centos-webpanel.com/cwp-el7-latest
sh cwp-el7-latest</li>
</ul>
如果下载链接失效，你可以使用以下下载链接代替:
<ul>
 	<li>CentOS 6: http://dl1.centos-webpanel.com/files/cwp2-latest
CentOS 7: http://dl1.centos-webpanel.com/files/cwp-el7-latest</li>
</ul>
最后重启VPS，CWP面板就安装成功了。

<img class="alignnone size-full wp-image-1584" src="http://hss5.com/wp-content/uploads/2018/12/aa03-1.gif" width="680" height="366" alt="" />
<h2>二、CentOS Web Panel设置</h2>
打开CentOS Web Panel的登录地址，然后输入你的VPS账号与密码，就可以登录到CentOS Web Panel了。

<img class="alignnone size-full wp-image-1585" src="http://hss5.com/wp-content/uploads/2018/12/aa04-1.gif" width="680" height="366" alt="" />

这是CentOS Web Panel的操作界面，仪表板、CWP设置、服务器设置、Apache设置、PHP设置、服务S-S-H、服务配置、用户帐户、DomainsPackages、SQL ServicesEmail、DNS功能、SecurityFile管理、插件、开发人员、MenuScript安装程序。（点击放大）

<img class="alignnone size-full wp-image-1586" src="http://hss5.com/wp-content/uploads/2018/12/aa05-1.gif" width="1280" height="624" alt="" />

CentOS Web Panel设置部分主要有SELinux、备份配置、恢复备份、CWP插件、支持论坛、CWP WIKI、Yum管理、重新启动服务器等，在编辑设置中可以为CWP设置IP地址、域名等等。

<img class="alignnone size-full wp-image-1587" src="http://hss5.com/wp-content/uploads/2018/12/aa06-1.gif" width="680" height="366" alt="" />

在备份设置中就可以设置CWP备份目录与定时备份了。

<img class="alignnone size-full wp-image-1588" src="http://hss5.com/wp-content/uploads/2018/12/aa08-1.gif" width="680" height="366" alt="" />

有备份的话，也就可以恢复备份了。

<img class="alignnone size-full wp-image-1589" src="http://hss5.com/wp-content/uploads/2018/12/aa09-1.gif" width="680" height="366" alt="" />

CentOS Web Panel的Yum管理中可以自己手动添加Yum源或者删除某一个失效的源。

&nbsp;

<img class="alignnone size-full wp-image-1590" src="http://hss5.com/wp-content/uploads/2018/12/aa10-1.gif" width="680" height="366" alt="" />
<h2>三、服务器设置</h2>
在服务器设置中主要有定时任务、修改Root密码、S-S-H Key生成器、修改日期时间、修改Hostname、分析磁盘空间等。

<img class="alignnone size-full wp-image-1591" src="http://hss5.com/wp-content/uploads/2018/12/aa11-1.gif" width="680" height="366" alt="" />

其中，Hostname就是CWP的后台登录地址，如果你想修改记得先把域名的DNS解析到该服务器上。

<img class="alignnone size-full wp-image-1592" src="http://hss5.com/wp-content/uploads/2018/12/aa12-1.gif" width="680" height="366" alt="" />

Disk Quota就是磁盘挂载与空间分配了。

<img class="alignnone size-full wp-image-1593" src="http://hss5.com/wp-content/uploads/2018/12/aa13-1.gif" width="680" height="366" alt="" />
<h2>四、Apache设置</h2>
CentOS Web Panel默认会安装Apache作为服务器引擎，在Apache设置中主要有选择WebServers、Apache配置、Apache状态、Apache重建、Apache Include Conf、编辑Apache vHosts、重建虚拟主机、Apache vHosts模板、Apache重定向、SSL证书管理器、Letsencrypt管理器、Tomcat管理器。

CentOS Web Panel提供了Apache Only、LiteSpeed Enterprise 、Apache &amp; Nginx Reverse Proxy 、Apache &amp; Varnish Cache、Apache &amp; Varnish Cache &amp; Nginx Reverse Proxy等几种模式可供选择。

<img class="alignnone size-full wp-image-1594" src="http://hss5.com/wp-content/uploads/2018/12/aa14-1.gif" width="680" height="366" alt="" />

在Apache redirects中可以设置端口或者网址301跳转，这样免去了你编辑配置文件了。

<img class="alignnone size-full wp-image-1595" src="http://hss5.com/wp-content/uploads/2018/12/aa15-1.gif" width="680" height="366" alt="" />

在SSL证书管理中可以管理各个虚拟主机上的SSL证书。（点击放大）

<img class="alignnone size-full wp-image-1596" src="http://hss5.com/wp-content/uploads/2018/12/aa16-1.gif" width="1280" height="555" alt="" />

同时也支持自动签发SSL证书。

<img class="alignnone size-full wp-image-1597" src="http://hss5.com/wp-content/uploads/2018/12/aa17-1.gif" width="680" height="366" alt="" />

CentOS Web Panel面板也有Letsencrypt证书自动签发功能。

<img class="alignnone size-full wp-image-1598" src="http://hss5.com/wp-content/uploads/2018/12/aa18-1.gif" width="680" height="366" alt="" />
<h2>五、PHP设置</h2>
在PHP设置中可以查看PHP info、PHP附加组件、PHP版本切换、PHP选择器、PHP编辑器、PHP.ini配置、FFMPEG安装。CentOS Web Panel支持PHP5.2 – 7.1等多达十几个版本的切换。

<img class="alignnone size-full wp-image-1599" src="http://hss5.com/wp-content/uploads/2018/12/aa19-1.gif" width="680" height="366" alt="" />

在PHP选择器可以安装PHP同时调整各个版本的PHP配置。

<img class="alignnone size-full wp-image-1600" src="http://hss5.com/wp-content/uploads/2018/12/aa20-1.gif" width="680" height="366" alt="" />

最后，CentOS Web Panel提供了PHP.ini在线编辑，编辑好了直接在线保存然后重启服务器即可。

<img class="alignnone size-full wp-image-1601" src="http://hss5.com/wp-content/uploads/2018/12/aa21-1.gif" width="680" height="366" alt="" />
<h2>六、其它功能介绍</h2>
在线控制台。在线控制台提供不少的实用Linux命令工具，例如Top、CPU、带宽监控、流量监控、网卡配置、实时监控、进程管理、Shell、命令等。

<img class="alignnone size-full wp-image-1602" src="http://hss5.com/wp-content/uploads/2018/12/aa22-1.gif" width="680" height="366" alt="" />

数据库管理。提供了PHPMyAdmin、MysqL管理器、MysqL配置、PosgreSQL安装、MongoDB Manager等。

<img class="alignnone size-full wp-image-1603" src="http://hss5.com/wp-content/uploads/2018/12/aa23-1.gif" width="680" height="366" alt="" />

免费邮局。CentOS Web Panel自带了邮局功能，你可以添加邮箱账号、查看邮件列表、检查rDNS、管理DKIM、SPF设置、防垃圾等。

<img class="alignnone size-full wp-image-1604" src="http://hss5.com/wp-content/uploads/2018/12/aa24-1.gif" width="680" height="366" alt="" />

这是CentOS Web Panel的在线邮局。

<img class="alignnone size-full wp-image-1605" src="http://hss5.com/wp-content/uploads/2018/12/aa25-1.gif" width="680" height="366" alt="" />

免费DNS域名解析系统。CentOS Web Panel自带了DNS，但是并不是直接安装的，而是直接整合了DNS在线平台的。

<img class="alignnone size-full wp-image-1606" src="http://hss5.com/wp-content/uploads/2018/12/aa26-1.gif" width="680" height="366" alt="" />

在线文件管理器。CentOS Web Panel自带的文件管理器非常强大。

<img class="alignnone size-full wp-image-1607" src="http://hss5.com/wp-content/uploads/2018/12/aa27-1.gif" width="1279" height="681" alt="" />

CentOS Web Panel可以直接管理VPS上的所有的文件。

<img class="alignnone size-full wp-image-1608" src="http://hss5.com/wp-content/uploads/2018/12/aa28-1.gif" width="680" height="366" alt="" />

整合WHMCS。CentOS Web Panel提供了WHMCS插件，如果你有WHMCS的话可以将CentOS Web Panel与WHMCS整合，具体参考：http://wiki.centos-webpanel.com/cwp-account-api。

<img class="alignnone size-full wp-image-1609" src="http://hss5.com/wp-content/uploads/2018/12/aa29-1.gif" width="680" height="366" alt="" />

想要了解更多关于WHMCS的知识，可以查看我的之前建立的专题：WHMCS从入门到精通。
<h2>七、总结</h2>
CentOS Web Panel各项功能非常强大，同时也比较复杂，所以在上手CentOS Web Panel时还是需要花费一定时间来研究学习。但是不管怎么说CentOS Web Panel确实是CentOS系统上不可多得的免费服务器管理面板。

CentOS Web Panel目前在国内用的人不是很多，并且也没有中文包，CentOS Web Panel（CWP）貌似没有卸载功能，如果不想用了看来只能是重装VPS系统了。另外，面板虽然有Nginx，但是Apache依然是最稳定的。

</div>