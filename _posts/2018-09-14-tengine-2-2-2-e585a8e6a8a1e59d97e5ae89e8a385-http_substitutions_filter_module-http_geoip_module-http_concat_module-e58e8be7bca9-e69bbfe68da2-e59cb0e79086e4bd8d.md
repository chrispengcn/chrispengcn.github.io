---
ID: 1363
post_title: 'Tengine 2.2.2 全模块安装  http_substitutions_filter_module  http_geoip_module http_concat_module 压缩 替换 地理位置 SSL'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/09/14/tengine-2-2-2-%e5%85%a8%e6%a8%a1%e5%9d%97%e5%ae%89%e8%a3%85-http_substitutions_filter_module-http_geoip_module-http_concat_module-%e5%8e%8b%e7%bc%a9-%e6%9b%bf%e6%8d%a2-%e5%9c%b0%e7%90%86%e4%bd%8d/'
published: true
post_date: 2018-09-14 09:12:46
---
&nbsp;

yum install gcc-c ++ pcre-devel zlib-devel make unzip libuuid-devel
yun install git

git clone git://github.com/yaoweibin/ngx_http_substitutions_filter_module.git

&nbsp;

cd /usr/src

&nbsp;

# wget http://geolite.maxmind.com/download/geoip/api/c/GeoIP.tar.gz

# tar -zxvf GeoIP.tar.gz

# cd GeoIP-1.4.8

# ./configure

# make; make install

# 刚才安装的库自动安装到 /usr/local/lib 下，所以这个目录需要加到动态链接配置里面以便运行相关程序的时候能自动绑定到这个 GeoIP 库：
# echo '/usr/local/lib' &gt; /etc/ld.so.conf.d/geoip.conf
#
# ldconfig

&nbsp;

&nbsp;
<pre>cd /usr/src

wget http://tengine.taobao.org/download/tengine-2.2.2.tar.gz

tar -xvzf tengine-2.2.2.tar.gz

cd tengine-2.2.2


./configure --prefix=/usr/local/nginx --with-http_gzip_static_module --with-http_geoip_module=shared --with-http_sub_module=shared --with-http_flv_module=shared --with-http_random_index_module=shared --with-http_access_module=shared --with-http_autoindex_module=shared --with-http_upstream_ip_hash_module=shared --with-http_upstream_least_conn_module=shared --with-http_concat_module=shared --add-module=/git/ngx_http_substitutions_filter_module --with-http_stub_status_module --with-http_ssl_module</pre>
&nbsp;

默认安装在/usr/local/nginx目录下，启动nginx，使用nginx -m并看不到动态模块，因为还没插入，所有的模块都是static的。

默认的动态模块在modules目录里面，这时候可动态插入的模块还没有被加入配置文件，我们可以手动加入/usr/local/nginx/conf/nginx.conf中
<pre>dso {
load ngx_http_access_module.so;
load ngx_http_autoindex_module.so;
load ngx_http_flv_module.so;
load ngx_http_geoip_module.so;
load ngx_http_random_index_module.so;
load ngx_http_sub_filter_module.so;
load ngx_http_upstream_ip_hash_module.so;
load ngx_http_upstream_least_conn_module.so;

}</pre>
启动nginx后使用nginx -m就可以看到懂爱模块都被标记被shared

Tengine动态开启模块试用

&nbsp;

&nbsp;
<blockquote>==========================GeoIP 模块设置============================================
# 下载Ip 数据库， 可放到 网站根目录 /var/www/GeoIP.dat
# wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz
#
# gunzip GeoIP.dat.gz

# vi /etc/nginx/nginx.conf

&nbsp;

h
http {
.
...
g
geoip_country /home/vpsee/GeoIP.dat;
f
fastcgi_param GEOIP_COUNTRY_CODE $geoip_country_code;
f
fastcgi_param GEOIP_COUNTRY_CODE3 $geoip_country_code3;
f
fastcgi_param GEOIP_COUNTRY_NAME $geoip_country_name;
.
...
}
}

&nbsp;

s
server {
.
...

location / {

root /home/vpsee/www;

if ($geoip_country_code = CN) {

root /home/vpsee/cn;

}

...

}
.
...
}
}</blockquote>