---
ID: 1290
post_title: 'nginx  magento 多域名配置'
author: chrispengcn
post_excerpt: |
  following is a summary of the tasks you must perform. More details are provided in the sections that follow.
  
  Define websites, stores, and store views in the Magento Admin.
  Create one virtual host per Magento website or store view.
  Pass the values of MAGE_RUN_TYPE and MAGE_RUN_CODE to the web server.
layout: post
permalink: 'http://hss5.com/2018/08/20/nginx-magento-%e5%a4%9a%e5%9f%9f%e5%90%8d%e9%85%8d%e7%bd%ae/'
published: true
post_date: 2018-08-20 23:24:17
---
following is a summary of the tasks you must perform. More details are provided in the sections that follow.
<ol>
 	<li>Define websites, stores, and store views in the Magento Admin.</li>
 	<li>Create one virtual host per Magento website or store view.</li>
 	<li>Pass the values of <code class="highlighter-rouge">MAGE_RUN_TYPE</code> and <code class="highlighter-rouge">MAGE_RUN_CODE</code> to the web server.</li>
</ol>
<pre><code>server {
    listen 8080 default;
    server_name 44.55.222.101; 
    root /var/www/html;
    location / {
        index index.php index.html index.htm;
        try_files $uri $uri/ @handler;
        expires 30d;
    }
    location ^~ /app/ { deny all; }
    location ^~ /includes/           { deny all; }
    location ^~ /lib/                { deny all; }
    location ^~ /media/downloadable/ { deny all; }
    location ^~ /pkginfo/            { deny all; }
    location ^~ /report/config.xml   { deny all; }
    location ^~ /var/                { deny all; }
    location /var/export/ {
        auth_basic           "Restricted";
        auth_basic_user_file htpasswd;
        autoindex            on;
    }
    location  /. {
        return 404;
    }
    location @handler {
        rewrite / /index.php;
    }
    location ~ .php/ {
        rewrite ^(.*.php)/ $1 last;
    }
    location ~ .php$ {
        if (!-e $request_filename) { rewrite / /index.php last; } 
        expires        off;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_param  HTTPS $fastcgi_https;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        fastcgi_param  MAGE_RUN_CODE website_new; 
        fastcgi_param  MAGE_RUN_TYPE website;
        include        fastcgi_params; 
    }
}</code></pre>