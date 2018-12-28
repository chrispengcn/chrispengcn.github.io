---
ID: 1461
post_title: nginx+php-fpm的session路径配置
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/24/nginxphp-fpm%e7%9a%84session%e8%b7%af%e5%be%84%e9%85%8d%e7%bd%ae/'
published: true
post_date: 2018-11-24 10:13:37
---
nginx+php-fpm的session路径配置

&nbsp;

不再是/etc/php.ini了，而是  /etc/php-fpm.d/www.conf
<pre>#vi /etc/php-fpm.d/www.conf</pre>
可以修改session路径
<pre>php_value[session.save_path] = /var/lib/php/session

</pre>
&nbsp;

设置权限和用户
<pre>chmod -R 777 /var/lib/php/session
chown -R nginx:nginx   /var/lib/php/session</pre>