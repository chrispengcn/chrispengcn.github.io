---
ID: 3387
post_title: >
  how to config multi domain wordpress
  with nginx
author: peng, chris
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/11/30/how-to-config-multi-domain-wordpress-with-nginx/
published: true
post_date: 2019-11-30 12:37:35
---
<pre>#vi /etc/nginx/conf.d/wordpress-multi.conf

server {
listen *:80;
listen [::]:80;
server_name 9wp.net yourwebsite.com;
return 301 http://www.$host$request_uri;
}


server {

listen *:80;
listen [::]:80;
server_name www.9wp.net www.yourwebsite.com;

root /var/www/vhosts/$host;
index index.html index.htm index.php;

# Load configuration files for the default server block.
include /etc/nginx/default.d/*.conf;
location / {
try_files $uri $uri/ /index.php?q=$uri&amp;$args;
}
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

location ~* \.(js|css|png|jpg|jpeg|gif|svg|ico|woff|woff2)$ {
expires 30d;
add_header Cache-Control "public, no-transform";
}

location ~* \.(pdf|css|html|js|swf)$ {
expires 2d;
}

location ~* .(eot|ttf|woff|svg|otf)$
{
add_header Access-Control-Allow-Origin *;
}

}


</pre>
&nbsp;

you can setup unlimited website in just on nging config file, that will save your time.