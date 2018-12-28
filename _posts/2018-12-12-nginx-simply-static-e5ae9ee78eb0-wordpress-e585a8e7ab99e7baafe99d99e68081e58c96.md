---
ID: 1707
post_title: >
  nginx + simply static 实现 wordpress
  全站纯静态化
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/12/nginx-simply-static-%e5%ae%9e%e7%8e%b0-wordpress-%e5%85%a8%e7%ab%99%e7%ba%af%e9%9d%99%e6%80%81%e5%8c%96/'
published: true
post_date: 2018-12-12 09:35:16
---
1， 安装simply static ， 生成纯静态文件到  /static/

2, 配置  wordpress.conf
<pre>server {
listen 80;
# listen [::]:80 default_server;
server_name www.cl-light.com.cn cl-light.com.cn; #这里是你自己的域名，如果没有，用本机ip
root /var/www/vhosts/cl-light.com.cn;  #这里是放wordpress的文件目录
index  index.php index.html index.htm;
# Load configuration files for the default server block.
include /etc/nginx/default.d/*.conf;

#location ^~ /?
#{
#try_files  $uri  $uri/ /index.php?q=$uri&amp;$args;
#}

location /index.php {
#try_files  /static/index.html $uri $uri/ /index.php?q=$uri&amp;$args;
}


location =/ {
#root /var/www/vhosts/cl-light.com.cn/static;
try_files  $uri /static/$uri/  $uri/ /index.php?q=$uri&amp;$args;
}

location / {
#root /var/www/vhosts/cl-light.com.cn/static;
try_files  $uri $uri/ /static/$uri/  /index.php?q=$uri&amp;$args;
}


#location /index.php {
#try_files  /static/index.html $uri $uri/ /index.php?q=$uri&amp;$args;
#}


location /wp-admin/{
root /var/www/vhosts/cl-light.com.cn;
try_files  $uri $uri/ /index.php?q=$uri&amp;$args;
}

location ~ \.php$ {
root /var/www/vhosts/cl-light.com.cn;
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

location ~ \.(gif|jpg|css|js|jpeg|png|bmp|ico)$ {

expires 1d;
}

}

</pre>