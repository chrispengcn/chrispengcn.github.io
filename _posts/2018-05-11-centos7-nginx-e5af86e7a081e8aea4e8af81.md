---
ID: 1188
post_title: 'centos7 nginx  密码认证'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/11/centos7-nginx-%e5%af%86%e7%a0%81%e8%ae%a4%e8%af%81/'
published: true
post_date: 2018-05-11 23:03:53
---
<pre>shell &gt; yum -y install httpd-tools  # 安装 htpasswd 工具

shell &gt; cd /etc/nginx/conf.d

shell &gt; htpasswd -c /etc/nginx/cert/pass.db wang # 创建认证用户 wang 并输入密码，添加用户时输入 htpasswd pass.db username 

</pre>
<blockquote>
<pre>shell &gt; vim /etc/nginx/conf.d/local.conf

server {
    listen       80;
    server_name  local.server.com;
    
    auth_basic "User Authentication";
    auth_basic_user_file /etc/nginx/cert/pass.db;
    
    location / {
        root   /data/www;
        index  index.html;
    }
}</pre>
</blockquote>