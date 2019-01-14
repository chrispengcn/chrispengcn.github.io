---
ID: 1390
post_title: Magento 1.9 安装在Nginx下的配置
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/09/20/magento-1-%e5%ae%89%e8%a3%85%e5%9c%a8nginx%e4%b8%8b%e7%9a%84%e9%85%8d%e7%bd%ae/'
published: true
post_date: 2018-09-20 20:10:23
---
fastcgi_cache_path /var/www/fastcgi_cache levels=1:2 keys_zone=MYAPP:500m inactive=10m;
fastcgi_cache_key "schemerequest_methodhostrequest_uri";

server {
listen 80;
server_name shopym.com www.shopym.com;
root /var/www/vhosts/shopym.com;

# Load configuration files for the default server block.
include /etc/nginx/default.d/*.conf;

location / {

&nbsp;

index index.php index.html index.htm;

if (-f $request_filename) {
expires 30d;
break;
}
if (!-e $request_filename) {
rewrite ^(.+)$ /index.php last;
}
}

&nbsp;

location ~ .php$ {
root /var/www/vhosts/shopym.com;
fastcgi_pass 127.0.0.1:9000;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME /var/www/vhosts/shopym.com$fastcgi_script_name;
include fastcgi_params;
fastcgi_cache MYAPP;
fastcgi_cache_valid 200 60m;
fastcgi_param MAGE_RUN_CODE chinese;
fastcgi_param MAGE_RUN_TYPE store;

}

location /app/etc {
deny all;
}

&nbsp;

error_page 404 /404.html;
location = /40x.html {
}

error_page 500 502 503 504 /50x.html;
location = /50x.html {
}
}

server {
listen 80;
server_name en.shopym.com;
root /var/www/vhosts/shopym.com;

# Load configuration files for the default server block.
include /etc/nginx/default.d/*.conf;

location / {

&nbsp;

index index.php index.html index.htm;

if (-f $request_filename) {
expires 30d;
break;
}
if (!-e $request_filename) {
rewrite ^(.+)$ /index.php last;
}
}

&nbsp;

location ~ .php$ {
root /var/www/vhosts/shopym.com;
fastcgi_pass 127.0.0.1:9000;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME /var/www/vhosts/shopym.com$fastcgi_script_name;
include fastcgi_params;
fastcgi_cache MYAPP;
fastcgi_cache_valid 200 60m;
fastcgi_param MAGE_RUN_CODE english;
fastcgi_param MAGE_RUN_TYPE store;

}

location /app/etc {
deny all;
}

&nbsp;

error_page 404 /404.html;
location = /40x.html {
}

error_page 500 502 503 504 /50x.html;
location = /50x.html {
}
}

server {
listen 80;
server_name kr.shopym.com;
root /var/www/vhosts/shopym.com;

# Load configuration files for the default server block.
include /etc/nginx/default.d/*.conf;

location / {

index index.php index.html index.htm;

if (-f $request_filename) {
expires 30d;
break;
}
if (!-e $request_filename) {
rewrite ^(.+)$ /index.php last;
}
}

&nbsp;

location ~ .php$ {
root /var/www/vhosts/shopym.com;
fastcgi_pass 127.0.0.1:9000;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME /var/www/vhosts/shopym.com$fastcgi_script_name;
include fastcgi_params;
fastcgi_cache MYAPP;
fastcgi_cache_valid 200 60m;
fastcgi_param MAGE_RUN_CODE korean;
fastcgi_param MAGE_RUN_TYPE store;

}

location /app/etc {
deny all;
}

&nbsp;

error_page 404 /404.html;
location = /40x.html {
}

error_page 500 502 503 504 /50x.html;
location = /50x.html {
}
}