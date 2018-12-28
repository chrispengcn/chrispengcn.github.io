---
ID: 1244
post_title: 'apache 2.4 配置  php-fpm'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/27/apache-2-4-%e9%85%8d%e7%bd%ae-php-fpm/'
published: true
post_date: 2018-05-27 16:54:37
---
vi /etc/httpd/conf/httpd.conf

文件末尾加上
<blockquote>&lt;VirtualHost *:80&gt;
DocumentRoot "/var/www/html"
ServerName www.yyenglish.cn
ServerAlias yyenglish.cn
ProxyRequests Off
ProxyPassMatch ^/(.*\.php)$ fcgi://127.0.0.1:9000/var/www/html/$1

&nbsp;

# If the php file doesn't exist, disable the proxy handler.
# This will allow .htaccess rewrite rules to work and
# the client will see the default 404 page of Apache
RewriteCond %{REQUEST_FILENAME} \.php$
RewriteCond %{DOCUMENT_ROOT}/%{REQUEST_URI} !-f
RewriteRule (.*) - [H=text/html]

&nbsp;

&lt;Directory "/var/www/html"&gt;
Options none
AllowOverride All
Require all granted
&lt;/Directory&gt;
&lt;/VirtualHost&gt;</blockquote>
systemctl restart httpd    // 重启 httpd 服务

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;