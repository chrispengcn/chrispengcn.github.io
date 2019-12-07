---
ID: 3405
post_title: >
  using simply static and wp rocket
  together to boost your website speed
author: peng, chris
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/12/08/using-simply-static-and-wp-rocket-together-to-boost-your-website-speed/
published: true
post_date: 2019-12-08 05:20:18
---
#https://www.hss5.com/2019/10/16/wp-super-cache-and-nginx-config/
#https://www.hss5.com/2019/09/06/wp-rocket%E9%85%8D%E5%90%88nginx%E5%AE%9E%E7%8E%B0%E7%BA%AF%E9%9D%99%E6%80%81%E5%8C%96%E5%8A%A0%E9%80%9Fwordpress%EF%BC%8Crocket-nginx/

now you can use the simply static and wp-rocket plugin together.   below code is edit base on wp supercache .  I modified it , and make it works on simply static and wp-rocket plugin.
<pre>server {
        listen *:80;
        listen [::]:80;
        server_name 9wp.net tripdir.com armstrong-mec.com;
        return 301 http://www.$host$request_uri;
}


server {

        listen *:80;
        listen [::]:80;
        server_name www.9wp.net www.tripdir.com www.armstrong-mec.com;

        root  /var/www/vhosts/$host;
        index index.html index.htm index.php;

# Rocket-Nginx configuration , you can remove below line , if don't installed the rocket-nginx
include /usr/local/nginx/rocket-nginx/default.conf;

# Load configuration files for the default server block.
include /etc/nginx/default.d/*.conf;
#location / {
#try_files $uri $uri/ /index.php?q=$uri&amp;$args;
#}
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


# WP Super Cache config.
# Designed to be included from a 'wordpress-ms-...' configuration file.
set $cache_uri $request_uri;

# POST 的 and get don't cache
if ($request_method = POST) {
        set $cache_uri 'null cache';
}

if ($query_string != "") {
        set $cache_uri 'null cache';
}

# dont't cache below file , dont't cache pictures , so you no need to generate image , that can save your disk place.
if ($request_uri ~* "(/wp-content/|/wp-admin/|/xmlrpc.php|/wp-(app|cron|login|register|mail).php|wp-.*.php|/feed/|index.php|wp-comments-popup.php|wp-links-opml.php|wp-locations.php|sitemap(_index)?.xml|[a-z0-9_-]+-sitemap([0-9]+)?.xml)") {
        set $cache_uri 'null cache';
}

#don't cache logged users
if ($http_cookie ~* "comment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_logged_in") {
        set $cache_uri 'null cache';
}


# use simply static html first then wp-rocket php.
location / {
        try_files /static/$cache_uri/index.html $uri $uri/ /index.php?q=$uri&amp;$args;
}
location = /favicon.ico { log_not_found off; access_log off; }
location = /robots.txt  { log_not_found off; access_log off; }
 # Cache static files for as long as possible


        location ~* \.(js|css|png|jpg|jpeg|gif|svg|ico|woff|woff2)$ {
                 expires 30d;
                 add_header Cache-Control "public, no-transform";
        }

        location ~*  \.(pdf|css|html|js|swf)$ {
                expires 2d;
        }

        location ~* .(eot|ttf|woff|svg|otf)$
          {
                add_header Access-Control-Allow-Origin *;
        }

}</pre>