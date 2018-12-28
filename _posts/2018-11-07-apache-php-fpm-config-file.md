---
ID: 1419
post_title: 'apache  php-fpm config file'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/11/07/apache-php-fpm-config-file/
published: true
post_date: 2018-11-07 09:52:48
---
vi /etc/httpd/conf.d/hss5.conf
<pre>&lt;VirtualHost *:8080&gt;
ServerAdmin admin@qq.com
DocumentRoot "/var/www/html"
ServerName www.hss5.com
ServerAlias  hss5.com
ErrorLog "logs/hss5.error.log"
CustomLog "logs/ hss5.common.log" common
ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://127.0.0.1:9000/var/www/html/$1
DirectoryIndex index.php index.html index.htm
&lt;Directory "/var/www/html"&gt;
Options Indexes FollowSymLinks MultiViews
AllowOverride All
Require all granted
&lt;/Directory&gt;
&lt;/VirtualHost&gt;</pre>