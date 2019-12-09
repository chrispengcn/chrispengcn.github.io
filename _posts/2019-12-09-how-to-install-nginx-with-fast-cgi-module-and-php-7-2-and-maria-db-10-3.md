---
ID: 3406
post_title: >
  how to install  nginx with fast-cgi
  module and php 7.2 and maria db 10.3?
author: peng, chris
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/12/09/how-to-install-nginx-with-fast-cgi-module-and-php-7-2-and-maria-db-10-3/
published: true
post_date: 2019-12-09 13:14:29
---
install  nginx with fast-cgi module and php 7.2 and maria db 10.3
<pre>#cat /usr/local/src/installlnmp.sh

yum -y update
yum -y install wget zip unzip gzip
cd /usr/local/src
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
yum -y install php72w-common php72w-fpm php72w-opcache php72w-gd php72w-mysqlnd php72w-mbstring php72w-pecl-redis php72w-pecl-memcached php72w-devel
systemctl enable php-fpm
systemctl start php-fpm
yum -y install gcc gcc-c++ autoconf automake zlib zlib-devel pcre-devel openssl openssl-devel
yum -y install nginx
wget http://nginx.org/download/nginx-1.15.7.tar.gz
tar zxvf nginx-1.15.7.tar.gz

#下载缓存清除 模块 ngx_cache_purge
wget http://labs.frickle.com/files/ngx_cache_purge-2.3.tar.gz
tar zxvf ngx_cache_purge-2.3.tar.gz

cd nginx-1.15.7.tar.gz
./configure --with-poll_module
--with-http_stub_status_module --with-http_ssl_module --add-module=../ngx_cache_purge-2.3 --with-http_geoip_module --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -pie' --with-http_addition_module
make
make install
systemctl enable nginx
systemctl start nginx
cd /etc/nginx/conf.d/
wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz
wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz
gzip -d GeoIP.dat.gz
gzip -d GeoLiteCity.dat.gz
echo "[mariadb]" &gt;&gt; /etc/yum.repos.d/MariaDB.repo
echo "name = MariaDB" &gt;&gt; /etc/yum.repos.d/MariaDB.repo
echo "baseurl = http://yum.mariadb.org/10.3/centos7-amd64" &gt;&gt; /etc/yum.repos.d/MariaDB.repo
echo "gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB" &gt;&gt; /etc/yum.repos.d/MariaDB.repo
echo "gpgcheck=1" &gt;&gt; /etc/yum.repos.d/MariaDB.repo
yum -y update
yum -y install mariadb mariadb-server
systemctl enable mariadb
systemctl start mariadb
# mysql_secure_installation
### php-fpm session ###
mkdir /var/lib/php/session
mkdir /var/lib/php/wsdlcache
chmod -R 777 /var/lib/php
chown -R apache:apache /var/lib/php</pre>