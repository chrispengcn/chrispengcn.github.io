---
ID: 1769
post_title: >
  nginx php-fpm fast-cgi gzip optimize
  config file
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/12/19/nginx-php-fpm-fast-cgi-gzip-config-file/
published: true
post_date: 2018-12-19 09:14:04
---
vi /etc/nginx/conf.d/gzipfcgi.conf
<pre>gzip on;
gzip_min_length 1k;
gzip_buffers 4 16k;
gzip_http_version 1.1;
gzip_comp_level 9;
gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php;
gzip_vary on;

fastcgi_connect_timeout 1600;
fastcgi_send_timeout 1600;
fastcgi_read_timeout 1600;
fastcgi_buffer_size 62k;
fastcgi_buffers 4 62k;
fastcgi_busy_buffers_size 62k;
fastcgi_temp_file_write_size 62k;


client_header_timeout 200s; #调大点
client_body_timeout 200s; #调大点
client_max_body_size 100m; #主要是这个参数，限制了上传文件大大小
client_body_buffer_size 256k;</pre>