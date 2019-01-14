---
ID: 1340
post_title: nginx magento 开启 fastcgi cache
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/08/31/nginx-magento-%e5%bc%80%e5%90%af-fastcgi-cache/'
published: true
post_date: 2018-08-31 15:54:12
---
<pre>

fastcgi_cache_path /var/www/fastcgi_cache levels=1:2 keys_zone=MYAPP:500m inactive=60m;
fastcgi_cache_key "schemerequest_methodhostrequest_uri";

server {
listen 80;
 server_name banqled.com www.banqled.com;
 root /var/www/vhosts/banqled.com;

# Load configuration files for the default server block.
 include /etc/nginx/default.d/*.conf;

location / {

root /var/www/vhosts/banqled.com;
index index.php index.html index.htm;

if (-f $request_filename) {
expires 30d;
break;
}
if (!-e $request_filename) {
rewrite ^(.+)$ /index.php last;
}



}



location ~ .php$ {
 root /var/www/vhosts/banqled.com;
 fastcgi_pass 127.0.0.1:9000;
 fastcgi_index index.php;
 fastcgi_param SCRIPT_FILENAME /var/www/vhosts/banqled.com/$fastcgi_script_name;
 include fastcgi_params;
 fastcgi_cache MYAPP;
 fastcgi_cache_valid 200 60m;
 }

location /app/etc {
deny all;
}



error_page 404 /404.html;
 location = /40x.html {
 }

error_page 500 502 503 504 /50x.html;
 location = /50x.html {
 }
}

server {
# listen 80 default_server;
# listen [::]:80 default_server;
listen 443 ssl;
#ssl off;

ssl_certificate /usr/local/nginx/cert/1528284481193.pem;
ssl_certificate_key /usr/local/nginx/cert/1528284481193.key;
ssl_session_timeout 5m;
ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;



server_name banqled.com www.banqled.com;
 root /var/www/vhosts/banqled.com;
#set $upstream http://ledstrips.cn:8080;

# Load configuration files for the default server block.
 include /etc/nginx/default.d/*.conf;

location / {

root /var/www/vhosts/banqled.com;
index index.php index.html index.htm;

if (-f $request_filename) {
expires 30d;
break;
}
if (!-e $request_filename) {
rewrite ^(.+)$ /index.php last;
}

proxy_cache my_cache;
proxy_pass http://www.banqled.com;
# sub_filter 'http://www.banqled.com' 'https://www.banqled.com";
# sub_filter_once off;
proxy_cache_use_stale error timeout http_500 http_502 http_503 http_504;

}



# location ~ .php$ {
 # root /var/www/vhosts/banqled.com;
 # fastcgi_pass 127.0.0.1:9000;
 # fastcgi_index index.php;
 # fastcgi_param SCRIPT_FILENAME /var/www/vhosts/banqled.com/$fastcgi_script_name;
 # include fastcgi_params;
 # fastcgi_cache MYAPP;
 # fastcgi_cache_valid 200 60m;
 # }

location /app/etc {
deny all;
}



error_page 404 /404.html;
 location = /40x.html {
 }

error_page 500 502 503 504 /50x.html;
 location = /50x.html {
 }
}</pre>