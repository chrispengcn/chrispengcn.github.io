---
ID: 3340
post_title: >
  How to install OpenLiteSpeed and PHP
  7.2/7.1/7.0/5.6 from LiteSpeed
  Repositories on Centos 7/6
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/11/25/how-to-install-openlitespeed-and-php-7-2-7-1-7-0-5-6-from-litespeed-repositories-on-centos-7-6/
published: true
post_date: 2019-11-25 17:43:56
---
We’re going to install OpenLiteSpeed on centos 7/6 server from litespeed repo. OpenLiteSpeed is the Open Source edition of LiteSpeed Web Server Enterprise. Both servers are actively developed and maintained by the same team, and are held to the same high-quality coding standard.<b> </b>OpenLiteSpeed contains all of the essential features found in LiteSpeed Enterprise, and represents our commitment to support the Open Source community. It is recommended to use Centos 7/6 for OpenLiteSpeed for stability.
<h3>Some Features :</h3>
<strong>Event-Driven Architecture</strong>
Fewer processes, less overhead, and enormous scalability. Keep your existing hardware.

<strong>Upgrade from Apache</strong>
OpenLiteSpeed is mod_rewrite compatible, so you can continue to use your current rewrite rules.

<strong>Friendly Admin Interfaces</strong>
OLS comes with a built-in WebAdmin GUI. Control panel support is available with CyberPanel.

<strong>Built for Speed and Security</strong>
Features Anti-DDoS connection and bandwidth throttling, ModSecurity v3 integration, and more.

<strong>Intelligent Cache Acceleration</strong>
Built-in full-page cache module is highly-customizable and efficient for an exceptional user experience.

<strong>PageSpeed Optimization</strong>
Automatically implement Google’s PageSpeed optimization system with the mod_pagespeed module.

<strong>PHP LiteSpeed SAPI</strong>
Native SAPI for PHP allows external applications written in PHP to run up to 50% faster.
<div class="google-auto-placed ap_container"><ins class="adsbygoogle adsbygoogle-noablate" data-ad-format="auto" data-ad-client="ca-pub-8448624395816410" data-adsbygoogle-status="done"><ins id="aswift_5_expand"><ins id="aswift_5_anchor"><iframe id="aswift_5" name="aswift_5" width="779" height="195" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" allowfullscreen="allowfullscreen" data-mce-fragment="1"></iframe></ins></ins></ins></div>
<strong>One-Click Installation</strong>
Install OpenLiteSpeed, MariaDB and WordPress on various operating systems with just one click.

<strong>WordPress Acceleration</strong>
Experience a measurable performance boost with OpenLiteSpeed and LSCache for WordPress.
<h1>STEP 1</h1>
<strong>Install repo :</strong>
<h3>CentOS 6:</h3>
<pre>yum install epel-release
rpm -ivh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el6.noarch.rpm
</pre>
<h3>CentOS 7:</h3>
<pre>yum install epel-release
rpm -ivh http://rpms.litespeedtech.com/centos/litespeed-repo-1.1-1.el7.noarch.rpm 
</pre>
<h1>STEP 2</h1>
Ensure you’ve <strong>removed</strong> any other <strong>http server (apache,nginx)</strong>

<strong>Install Latest/recent version of  OpenLiteSpeed From yum :</strong>
<pre>yum install openlitespeed</pre>
<h1>STEP 3</h1>
<strong>Install PHP from litespeed repository :-</strong>
<h3><strong>Install dependencies :</strong></h3>
<pre>yum -y install gcc make gcc-c++ cpp kernel-headers.x86_64 libxml2-devel openssl-devel bzip2-devel libjpeg-devel libpng-devel freetype-devel openldap-devel postgresql-devel aspell-devel net-snmp-devel libxslt-devel libc-client-devel libicu-devel gmp-devel curl-devel libmcrypt libmcrypt-devel pcre-devel sqlite-devel db4-devel enchant-devel libXpm-devel mysql-devel readline-devel libedit-devel recode-devel libtidy-devel libtool-ltdl-devel  
</pre>

<hr />

<h3>To install PHP 7.2 :</h3>
<strong>This command will install php 7.2</strong>
<pre>yum install lsphp72 lsphp72-bcmath lsphp72-common lsphp72-dba lsphp72-dbg lsphp72-devel lsphp72-enchant lsphp72-gd lsphp72-gmp lsphp72-imap lsphp72-intl lsphp72-json lsphp72-ldap lsphp72-mbstring lsphp72-mysqlnd lsphp72-odbc lsphp72-opcache lsphp72-pdo lsphp72-pear lsphp72-pecl-apcu lsphp72-pecl-apcu-devel lsphp72-pecl-apcu-panel lsphp72-pecl-igbinary lsphp72-pecl-igbinary-devel lsphp72-pecl-mcrypt lsphp72-pecl-memcache lsphp72-pecl-memcached lsphp72-pecl-msgpack lsphp72-pecl-msgpack-devel lsphp72-pecl-redis lsphp72-pgsql lsphp72-process lsphp72-pspell lsphp72-recode lsphp72-snmp lsphp72-soap lsphp72-tidy lsphp72-xml lsphp72-xmlrpc lsphp72-zip  
</pre>
<strong>Then we need to symlink the lib folder</strong>
<pre>ln -s /usr/local/lsws/lsphp72/lib64 /usr/local/lsws/lsphp72/lib
</pre>

<hr />

<h3>To install PHP 7.1 :</h3>
<strong>This command will install php 7.1</strong>
<pre>lsphp71 lsphp71-mcrypt lsphp71-bcmath lsphp71-common lsphp71-dba lsphp71-dbg lsphp71-devel lsphp71-enchant lsphp71-gd lsphp71-gmp lsphp71-imap lsphp71-intl lsphp71-json lsphp71-ldap lsphp71-mbstring lsphp71-mysqlnd lsphp71-odbc lsphp71-opcache lsphp71-pdo lsphp71-pear lsphp71-pecl-apcu lsphp71-pecl-apcu-devel lsphp71-pecl-apcu-panel lsphp71-pecl-igbinary lsphp71-pecl-igbinary-devel lsphp71-pecl-mcrypt lsphp71-pecl-memcache lsphp71-pecl-memcached lsphp71-pecl-msgpack lsphp71-pecl-msgpack-devel lsphp71-pecl-redis lsphp71-pgsql lsphp71-process lsphp71-pspell lsphp71-recode lsphp71-snmp lsphp71-soap lsphp71-tidy lsphp71-xml lsphp71-xmlrpc lsphp71-zip    
</pre>
<strong>Then we need to symlink the lib folde</strong>r
<pre>ln -s /usr/local/lsws/lsphp71/lib64 /usr/local/lsws/lsphp71/lib
</pre>

<hr />

<h3>To install PHP 7.0 :</h3>
<strong>This command will install php 7.0</strong>
<pre>lsphp70 lsphp70-mcrypt lsphp70-bcmath lsphp70-common lsphp70-dba lsphp70-dbg lsphp70-devel lsphp70-enchant lsphp70-gd lsphp70-gmp lsphp70-imap lsphp70-intl lsphp70-json lsphp70-ldap lsphp70-mbstring lsphp70-mysqlnd lsphp70-odbc lsphp70-opcache lsphp70-pdo lsphp70-pear lsphp70-pecl-apcu lsphp70-pecl-apcu-devel lsphp70-pecl-apcu-panel lsphp70-pecl-igbinary lsphp70-pecl-igbinary-devel lsphp70-pecl-mcrypt lsphp70-pecl-memcache lsphp70-pecl-memcached lsphp70-pecl-msgpack lsphp70-pecl-msgpack-devel lsphp70-pecl-redis lsphp70-pgsql lsphp70-process lsphp70-pspell lsphp70-recode lsphp70-snmp lsphp70-soap lsphp70-tidy lsphp70-xml lsphp70-xmlrpc lsphp70-zip
</pre>
<strong>Then we need to symlink the lib folder</strong>
<pre>ln -s /usr/local/lsws/lsphp70/lib64 /usr/local/lsws/lsphp70/lib
</pre>

<hr />

<h3>To install PHP 5.6 :</h3>
<strong>This command will install php 5.6</strong>
<pre>lsphp56 lsphp56-mcrypt lsphp56-bcmath lsphp56-common lsphp56-dba lsphp56-dbg lsphp56-devel lsphp56-enchant lsphp56-gd lsphp56-gmp lsphp56-imap lsphp56-intl lsphp56-json lsphp56-ldap lsphp56-mbstring lsphp56-mysqlnd lsphp56-odbc lsphp56-opcache lsphp56-pdo lsphp56-pear lsphp56-pecl-apcu lsphp56-pecl-apcu-devel lsphp56-pecl-apcu-panel lsphp56-pecl-igbinary lsphp56-pecl-igbinary-devel lsphp56-pecl-mcrypt lsphp56-pecl-memcache lsphp56-pecl-memcached lsphp56-pecl-msgpack lsphp56-pecl-msgpack-devel lsphp56-pecl-redis lsphp56-pgsql lsphp56-process lsphp56-pspell lsphp56-recode lsphp56-snmp lsphp56-soap lsphp56-tidy lsphp56-xml lsphp56-xmlrpc lsphp56-zip
</pre>
<strong>Then we need to symlink the lib folder :</strong>
<pre>ln -s /usr/local/lsws/lsphp56/lib64 /usr/local/lsws/lsphp56/lib
</pre>
<h2>Visit this tutorial :</h2>
<h2 class="entry-title grid-title"><a href="https://www.mysterydata.com/how-to-configure-php-7-2-7-1-7-0-with-openlitespeed/">How to Configure PHP 7.2/7.1/7.0 with OpenLiteSpeed</a></h2>

<hr />

<h1>STEP 4</h1>
<h3><strong>Firewall configurations :</strong></h3>
if you’ve firewaall installed ensure this ports are open :
<ul>
 	<li><strong>80</strong></li>
 	<li><strong>443</strong></li>
 	<li><strong>8080</strong></li>
 	<li><strong>7080</strong></li>
</ul>
For <strong>Iptables</strong> use this commands to open the ports :
<pre>iptables -I INPUT -p tcp --dport 80 -j ACCEPT
iptables -I INPUT -p tcp --dport 443 -j ACCEPT
iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
iptables -I INPUT -p tcp --dport 7080 -j ACCEPT
</pre>
<h1>STEP 5</h1>
<h3>Start OpenLiteSpeed and Login to Dashboard :</h3>
<strong>To start OpenLiteSpeed:</strong>
<pre>service lsws start
</pre>
<strong>To stop and restart :</strong>
<pre>service lsws stop
service lsws restart
</pre>
<strong>Now we need to reset the OpenLiteSpeed admin password :</strong>
<pre>cd /usr/local/lsws/admin/misc
./admpass.sh
</pre>
<strong>when prompting for username leave it blank eg.:</strong>
<pre>[root@mysterydata ~]# cd /usr/local/lsws/admin/misc
[root@mysterydata misc]# ./admpass.sh

Please specify the user name of administrator.
This is the user name required to login the administration Web interface.

User name [admin]:

Please specify the administrator's password.
This is the password required to login the administration Web interface.

Password:
Retype password:
Administrator's username/password is updated successfully!
</pre>
<h1>STEP 6</h1>
<strong>Login to OpenLiteSpeed admin panel :</strong>
<pre>https://1.2.3.4:7080
</pre>
replace 1.2.3.4 with your server ip.
<h1>STEP 7</h1>
<h3 id="configure-port-">Configure Port 80 (http) and 443 (https)</h3>
The default http port for openLiteSpeed is 8080, it’s used to receive the client requests. In this step, we will change the port to 80 from the openLiteSpeed management GUI.

On the left side, go to the <span class="system">“Listeners”</span> section to see the listeners configuration. You will see the default listeners with port 8080. Click <span class="system">“view”</span> zoom icon to see details configuration. Now click <span class="system">“Edit”</span>.
<pre>IP Address: ANY
Port 80</pre>
Change the port to 80 and save the configuration.

<img class="alignnone size-full wp-image-3341" src="https://www.hss5.com/wp-content/uploads/2019/11/2018-06-19_183723.png" width="765" height="437" alt="" />

<strong>For SSL 443</strong> follow this official documentation : <a href="https://openlitespeed.org/mediawiki/index.php/Help:SSL_Setup" target="_blank" rel="noopener noreferrer">https://openlitespeed.org/mediawiki/index.php/Help:SSL_Setup</a>

<strong>thats it you’ve successfully installed OpenLiteSpeed</strong>

&nbsp;

https://www.mysterydata.com/how-to-install-openlitespeed-and-php-7-2-7-1-7-0-5-6-from-litespeed-repositories-on-centos-7-6/