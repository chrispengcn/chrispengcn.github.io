---
ID: 1174
post_title: >
  https and wordpress config for
  nginx.conf
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/05/10/centos7-nginx-and-https-and-wordpress/
published: true
post_date: 2018-05-10 00:50:30
---
<blockquote>server {
listen 80 default backlog=2048;
listen 443 ssl;
# listen [::]:80 default_server;
server_name www.hss5.com hss5.com; #这里是你自己的域名，如果没有，用本机ip
root /www/hss5.com; #这里是放wordpress的文件目录
index index.php index.html;

ssl_certificate  cert/hss5._com.crt;
ssl_certificate_key cert/hss5.com.key;
ssl_session_timeout 5m;
ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;

# Load configuration files for the default server block.
include /etc/nginx/default.d/*.conf;
location / {
try_files $uri $uri/ /index.php?$args;

}

# Add trailing slash to */wp-admin requests.
rewrite /wp-admin$ $scheme://$host$uri/ permanent;

location ~ .php$ {
fastcgi_pass 127.0.0.1:9000;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
include fastcgi_params;
}
error_page 404 /404.html;
location = /40x.html {
}
error_page 500 502 503 504 /50x.html;
location = /50x.html {
}
}</blockquote>