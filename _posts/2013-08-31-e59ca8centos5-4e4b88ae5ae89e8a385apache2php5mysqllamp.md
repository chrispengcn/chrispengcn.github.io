---
ID: 251
post_title: >
  在CentOS5.4上安装Apache2+PHP5+MySQL(LAMP)
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/08/31/%e5%9c%a8centos5-4%e4%b8%8a%e5%ae%89%e8%a3%85apache2php5mysqllamp/'
published: true
post_date: 2013-08-31 21:32:56
---
<p>在<strong>CentOS</strong>5.4上<strong>安装</strong>Apache2+PHP5+MySQL(<strong>LAMP</strong>) <p><strong>1.注意事项</strong>：在这篇教程中，我将使用的主机IP 地址是192.168.0.100.这些设置可能与你的机器不同，因此你需要在合适的地方更换下。 <p><strong>2.安装MySQL5.0</strong> <p>我们通过执行下面的命令来安装MySQl：</p><pre><ol><li>yum install mysql mysql-server&nbsp; <li>&nbsp;</li></ol></pre>
<p>然后我们为MySQL创建系统启动快捷键(这样的话，MySQL就会在系统启动的时候自动启动)并且启动MySQL服务器：</p><pre><ol><li>chkconfig –levels 235 mysqld on&nbsp; <li>&nbsp;<li>/etc/init.d/mysqld start&nbsp; <li>&nbsp;</li></ol></pre>
<p>运行</p><pre><ol><li>mysqladmin -u root password yourrootsqlpassword&nbsp; <li>&nbsp;<li>mysqladmin -h server1.example.com -u root password yourrootsqlpassword&nbsp; <li>&nbsp;</li></ol></pre>
<p>来为root用户设置一个密码(否则的话任何人都可以访问你的MySQL数据库!)。
<p><strong>3安装Apache2</strong>
<p>Apache2 是CentOS的一个可供选择的包，因此我们可以使用下列命令安装它：</p><pre><ol><li>yum install httpd&nbsp; <li>&nbsp;</li></ol></pre>
<p>现在配置你的系统使得Apache可以自动启动。。。</p><pre><ol><li>chkconfig –levels 235 httpd on&nbsp; <li>&nbsp;</li></ol></pre>
<p>… 并且启动Apache</p><pre><ol><li>/etc/init.d/httpd start&nbsp; <li>&nbsp;</li></ol></pre>
<p>现在你可以在你的浏览器中转到http://192.168.0.114,你应该看到Apache2的预留页：
<p><a href="http://images.51cto.com/files/uploadimg/20110311/1721240.png"><img border="0" alt="CentOS/安装/LAMP" src="http://images.51cto.com/files/uploadimg/20110311/1721240.png" width="498" height="398"></a></p><pre><ol><li>apache preloadpage&nbsp; <li>&nbsp;<li>apache preloadpage&nbsp; <li>&nbsp;</li></ol></pre>
<p>在CentOS里Apache的默认文档路径的位置是在/var/www/html，配置文件的路径是/etc/httpd/conf/httpd.conf。其他的配置存储在/etc/httpd/conf.d/ 文件夹里。
<p><strong>4 安装PHP5</strong>
<p>我们可以使用下列命令来安装PHP5和Apache的PHP5模块：</p><pre><ol><li>yum install php&nbsp; <li>&nbsp;</li></ol></pre>
<p>然后我们必须重新启动Apache:</p><pre><ol><li>/etc/init.d/httpd start&nbsp; <li>&nbsp;</li></ol></pre>
<p><strong>5测试PHP5</strong>
<p>获取PHP5安装的一些信息
<p>网站的默认文档的路径是/var/www/html.我们可以在这个目录里创建一个简单的php文件(info.php)并且在浏览器中调用。这文件将会显示很多关于PHP安装时候的有用的细节，例如PHP的安装的版本。
<p>vi /var/www/html/info.php</p><pre><ol><li>phpinfo();&nbsp; <li>&nbsp;<li>?&gt; <li>&nbsp;</li></ol></pre>
<p>现在我们可以再浏览器中访问这个文件(例如http://192.168.0.114/info.php)：
<p><a href="http://images.51cto.com/files/uploadimg/20110311/1721241.png"><img border="0" alt="CentOS/安装/LAMP" src="http://images.51cto.com/files/uploadimg/20110311/1721241.png" width="498" height="398"></a></p><pre><ol><li>phpinfo&nbsp; <li>&nbsp;<li>phpinfo&nbsp; <li>&nbsp;</li></ol></pre>
<p>正如你所看到的，PHP5现在正在工作，正如Server API这一行中显示的一样，它是工作在Apache 2.0 Handler模式下。如果你向下滑动的话，你将会看到所有的模块都可以在PHP5中使用了，MySQL并没有在这里被列出来，这也就意味着PHP5并不支持MySQL。
<p><strong>6 使得PHP5支持MySQL</strong>
<p>要使得在PHP中支持MySQL，我们可以安装 php-mysql这个包。最好的办法是安装一些其他的PHP5模块，这些模块可能其他应用程序会用到。你可以使
<p>用search命令寻找可用的PHP5模块：</p><pre><ol><li>yum search php&nbsp; <li>&nbsp;</li></ol></pre>
<p>选择你所需要的包，然后通过下列命令安装他们：</p><pre><ol><li>yum install php-mysql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc&nbsp; <li>&nbsp;</li></ol></pre>
<p>现在重新启动Apache2</p><pre><ol><li>/etc/init.d/httpd restart&nbsp; <li>&nbsp;</li></ol></pre>
<p>现在在你的浏览器中重新加载http://192.168.0.114/info.php 这个页面，并再次查看模块部分，你现在就能看到多了很多模块，包括我们刚刚安装的MySQL模块。
<p><a href="http://images.51cto.com/files/uploadimg/20110311/1721242.png"><img border="0" alt="CentOS/安装/LAMP" src="http://images.51cto.com/files/uploadimg/20110311/1721242.png" width="498" height="398"></a></p><pre><ol><li>mysqlmodule&nbsp; <li>&nbsp;<li>mysql module&nbsp; <li>&nbsp;</li></ol></pre>
<p><strong>7 phpMyAdmin</strong>
<p>phpMyAdmin是一款MySQL数据库web化的管理工具。
<p>第一步我们先使我们的CentOS支持RPMforge repository，因为phpMyAdmin并不在CentOS5.3官方的依赖包里：
<p>对于 x86_64 系统:</p><pre><ol><li>wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.3.6-1.el5.rf.x86_64.rpm&nbsp; <li>&nbsp;<li>rpm -Uvh rpmforge-release-0.3.6-1.el5.rf.x86_64.rpm&nbsp; <li>&nbsp;</li></ol></pre>
<p>对于 i386系统</p><pre><ol><li>wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.3.6-1.el5.rf.i386.rpm&nbsp; <li>&nbsp;<li>rpm -Uvh rpmforge-release-0.3.6-1.el5.rf.i386.rpm&nbsp; <li>&nbsp;</li></ol></pre>
<p>现在你就可以使用下列命令来安装phpMyAdmin了:</p><pre><ol><li>yum install phpmyadmin&nbsp; <li>&nbsp;</li></ol></pre>
<p>现在我们配置下phpMyAdmin。我们改下Apache的配置文件，使的 phpMyAdmin不单单是本机访问。</p><pre><ol><li>vi /etc/httpd/conf.d/phpmyadmin.conf&nbsp; <li>&nbsp;<li>#&nbsp; <li>&nbsp;<li># Web application to manage MySQL&nbsp; <li>&nbsp;<li>#&nbsp; <li>&nbsp;<li>#&nbsp; <li>&nbsp;<li># Order Deny,Allow&nbsp; <li>&nbsp;<li># Deny from all&nbsp; <li>&nbsp;<li># Allow from 127.0.0.1&nbsp; <li>&nbsp;<li>#&nbsp; <li>&nbsp;<li>Alias /phpmyadmin /usr/share/phpmyadmin&nbsp; <li>&nbsp;<li>Alias /phpMyAdmin /usr/share/phpmyadmin&nbsp; <li>&nbsp;<li>Alias /mysqladmin /usr/share/phpmyadmin&nbsp; <li>&nbsp;</li></ol></pre>
<p>下面我们改变下phpMyAdmin的认证方式，从cookie改成http：</p><pre><ol><li>vi /usr/share/phpmyadmin/config.inc.php&nbsp; <li>&nbsp;<li>[...]&nbsp; <li>&nbsp;<li>/* Authentication type */&nbsp; <li>&nbsp;<li>$cfg['Servers'][$i]['auth_type'] = ‘http’;&nbsp; <li>&nbsp;<li>[...]&nbsp; <li>&nbsp;</li></ol></pre>
<p>最后，你就可以通过http://192.168.0.114/phpmyadmin/当问phpMyAdmin了：
<p><a href="http://images.51cto.com/files/uploadimg/20110311/1721243.png"><img border="0" alt="CentOS/安装/LAMP" src="http://images.51cto.com/files/uploadimg/20110311/1721243.png" width="498" height="398"></a></p><pre><ol><li>phpmyadmin&nbsp; <li>&nbsp;<li>phpmyadmin&nbsp; </li></ol></pre>
<p>【编辑推荐】
<p><a href="http://os.51cto.com/art/201103/248776.htm">CentOS上安装LAMP(适用于所有VPS)</a>
<p><a href="http://os.51cto.com/art/201103/248715.htm">Centos安装配置LAMP的扩展</a>
<p><a href="http://os.51cto.com/art/201103/248653.htm">CentOS下安装LAMP的方法</a>