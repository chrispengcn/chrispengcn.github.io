---
ID: 1694
post_title: nginx使用geoip判断国家
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/10/nginx%e4%bd%bf%e7%94%a8geoip%e5%88%a4%e6%96%ad%e5%9b%bd%e5%ae%b6/'
published: true
post_date: 2018-12-10 21:24:19
---
进入目录/etc/nginx/conf.d/ ，下载并解压城市和国家数据文件
<pre>＃cd /etc/nginx/conf.d/

＃sudo wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz 
＃sudo wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz

＃sudo gzip -d GeoIP.dat.gz 
＃sudo gzip -d GeoLiteCity.dat.gz

</pre>
&nbsp;

wget http://nginx.org/download/nginx-1.15.7.tar.gz

# tar zxvf <a href="http://nginx.org/download/nginx-1.15.7.tar.gz">nginx-1.15.7.tar.gznginx-0.9.6.tar.gz</a>

# cd http://nginx.org/download/nginx-1.15.7.tar.gz

# ./configure
<pre class="code">--with-poll_module \
--with-http_stub_status_module --with-http_ssl_module \
--with-http_geoip_module --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -pie'</pre>
vi /etc/nginx/12530.top
<pre>geoip_country /etc/nginx/conf.d/GeoIP.dat;
geoip_city /etc/nginx/conf.d/GeoLiteCity.dat;

server{

listen 80;
listen 443 ssl;

ssl off;

server_name 12530.top www.12530.top;

 index index.html index.htm;
 ssl_certificate  /etc/nginx/ssl/www.12530.top.pem;
 ssl_certificate_key  /etc/nginx/ssl/www.12530.top.key;
 ssl_session_timeout 5m;
 ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
 ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
 ssl_prefer_server_ciphers on;


location /{
    if ($geoip_country_code = CN){
        proxy_pass https://www.xx.com;
    }


    if ($geoip_country_code != CN){
        rewrite  ^/(.*)$     https://www.xx.com/$1 permanent;
    }

}

}</pre>