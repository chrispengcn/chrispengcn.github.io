---
ID: 1225
post_title: 'prestashop 1.6  rewrite config on nginx 1.14'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/05/18/prestashop-1-6-rewrite-config-on-nginx-1-14/
published: true
post_date: 2018-05-18 22:59:21
---
vi /etc/nginx/conf.d/prestashop.conf
<blockquote>server {
listen 80;
#listen [::]:80; # Uncomment this line if you also want to enable IPv6 support
server_name chinaredstone.net www.chinaredstone.net;
root /var/www/html;
# access_log /var/log/nginx/example.access.log;
# error_log /var/log/nginx/example.error.log;

index index.php index.html; # Letting nginx know which files to try when requesting a folder
location = /favicon.ico {
log_not_found off; # PrestaShop by default does not provide a favicon.ico
access_log off; # Disable logging to prevent excessive log sizes
}
location = /robots.txt {
auth_basic off; # Whatever happens, always let bots know about your policy
allow all;
log_not_found off; # Prevent excessive log size
access_log off;
}

# Deny all attempts to access hidden files such as .htaccess, .htpasswd, .DS_Store (Mac).
location ~ /\. {
deny all;
access_log off;
log_not_found off;
}

# Gzip Settings

gzip on;
gzip_disable "msie6"; # Do people still use Internet Explorer 6? In that case, disable gzip and hope for the best!
gzip_vary on; # Also compress content with other MIME types than "text/html"
gzip_types application/json text/css application/javascript; # We only want to compress json, css and js. Compressing images and such isn't worth it
gzip_proxied any;
gzip_comp_level 6; # Set desired compression ratio, higher is better compression, but slower
gzip_buffers 16 8k; # Gzip buffer size
gzip_http_version 1.0; # Compress every type of HTTP request

# PrestaShop
rewrite ^/api/?(.*)$ /webservice/dispatcher.php?url=$1 last;
rewrite ^/([0-9])(\-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+\.jpg$ /img/p/$1/$1$2$3.jpg last;
rewrite ^/([0-9])([0-9])(\-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+\.jpg$ /img/p/$1/$2/$1$2$3$4.jpg last;
rewrite ^/([0-9])([0-9])([0-9])(\-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+\.jpg$ /img/p/$1/$2/$3/$1$2$3$4$5.jpg last;
rewrite ^/([0-9])([0-9])([0-9])([0-9])(\-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+\.jpg$ /img/p/$1/$2/$3/$4/$1$2$3$4$5$6.jpg last;
rewrite ^/([0-9])([0-9])([0-9])([0-9])([0-9])(\-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+\.jpg$ /img/p/$1/$2/$3/$4/$5/$1$2$3$4$5$6$7.jpg last;
rewrite ^/([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])(\-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+\.jpg$ /img/p/$1/$2/$3/$4/$5/$6/$1$2$3$4$5$6$7$8.jpg last;
rewrite ^/([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])(\-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+\.jpg$ /img/p/$1/$2/$3/$4/$5/$6/$7/$1$2$3$4$5$6$7$8$9.jpg last;
rewrite ^/([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])(\-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+\.jpg$ /img/p/$1/$2/$3/$4/$5/$6/$7/$8/$1$2$3$4$5$6$7$8$9$10.jpg last;
rewrite ^/c/([0-9]+)(\-[\.*_a-zA-Z0-9-]*)(-[0-9]+)?/.+\.jpg$ /img/c/$1$2$3.jpg last;
rewrite ^/c/([a-zA-Z_-]+)(-[0-9]+)?/.+\.jpg$ /img/c/$1$2.jpg last;
rewrite ^/images_ie/?([^/]+)\.(jpe?g|png|gif)$ /js/jquery/plugins/fancybox/images/$1.$2 last;
rewrite ^/order$ /index.php?controller=order last;
# location / {     这一行必须去掉   否则     /zh/index.php 找不到
if (!-e $request_filename){
rewrite ^(.*)$ /index.php?s=$1 last; break;
}
# }

location ~ \.php$ {
root /var/www/html; #指定php的根目录
fastcgi_pass 127.0.0.1:9000;#php-fpm的默认端口是9000
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME /var/www/html$fastcgi_script_name;
include fastcgi_params;
}

}</blockquote>
&nbsp;

&nbsp;