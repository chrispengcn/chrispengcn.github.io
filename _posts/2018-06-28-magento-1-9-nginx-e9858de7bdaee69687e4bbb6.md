---
ID: 1259
post_title: magento 1.9 nginx 配置文件
author: chrispengcn
post_excerpt: |
  文件名为：magento.conf（下载），将其放在 /usr/local/nginx/conf/ 文件夹下
  然后在 /usr/local/nginx/conf/vhost/www.yourname.com.conf 中将include none.conf; 换成include magento.conf;即可。
layout: post
permalink: 'http://hss5.com/2018/06/28/magento-1-9-nginx-%e9%85%8d%e7%bd%ae%e6%96%87%e4%bb%b6/'
published: true
post_date: 2018-06-28 17:52:56
---
文件名为：magento.conf（<a href="http://www.linodeclub.com/download/magento.zip" target="_blank" rel="nofollow noopener noreferrer">下载</a>），将其放在 /usr/local/nginx/conf/ 文件夹下
然后在 /usr/local/nginx/conf/vhost/www.yourname.com.conf 中将include none.conf; 换成include magento.conf;即可。
<blockquote>
<pre><code class="language-plain">location / {
        index index.html index.php; ## Allow a static html file to be shown first
        try_files $uri $uri/ @handler; ## If missing pass the URI to Magento's front handler
        expires 30d; ## Assume all files are cachable
    }

    ## These locations would be hidden by .htaccess normally
    location /app/                { deny all; }
    location /includes/           { deny all; }
    location /lib/                { deny all; }
    location /media/downloadable/ { deny all; }
    location /pkginfo/            { deny all; }
    location /report/config.xml   { deny all; }
    location /var/                { deny all; }

    location /var/export/ { ## Allow admins only to view export folder
        auth_basic           "Restricted"; ## Message shown in login window
        auth_basic_user_file htpasswd; ## See /etc/nginx/htpassword
        autoindex            on;
    }
      location  /. { ## Disable .htaccess and other hidden files
        return 404;
    }

    location @handler { ## Magento uses a common front handler
        rewrite / /index.php;
    }

    location ~ .php/ { ## Forward paths like /js/index.php/x.js to relevant handler
        rewrite ^(.*.php)/ $1 last;
    }</code></pre>
</blockquote>