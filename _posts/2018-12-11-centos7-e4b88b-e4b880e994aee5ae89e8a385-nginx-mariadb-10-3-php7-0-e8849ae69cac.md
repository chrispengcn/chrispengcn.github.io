---
ID: 1697
post_title: 'Centos7  下 一键安装 nginx, MariaDB 10.3, php7.0 &#8211; LNMP安装脚本'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/11/centos7-%e4%b8%8b-%e4%b8%80%e9%94%ae%e5%ae%89%e8%a3%85-nginx-mariadb-10-3-php7-0-%e8%84%9a%e6%9c%ac/'
published: true
post_date: 2018-12-11 09:44:30
---
Centos7  下 一键安装 nginx, MariaDB 10.3, php7.0 脚本

vi /var/local/src/installlnmp.sh

&nbsp;
<pre>yum -y update
yum -y install wget zip unzip gzip
cd /usr/local/src

rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
yum -y install php70w-common php70w-fpm php70w-opcache php70w-gd php70w-mysqlnd php70w-mbstring php70w-pecl-redis php70w-pecl-memcached php70w-devel

systemctl enable php-fpm
systemctl start php-fpm

yum -y install gcc gcc-c++ autoconf automake zlib zlib-devel pcre-devel openssl openssl-devel
yum -y install nginx
wget http://nginx.org/download/nginx-1.15.7.tar.gz
tar zxvf nginx-1.15.7.tar.gz
cd nginx-1.15.7.tar.gz
./configure --with-poll_module 
--with-http_stub_status_module --with-http_ssl_module 
--with-http_geoip_module --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -pie' --with-http_addition_module

make
make install

systemctl enable nginx
systemctl start nginx

cd /etc/nginx/conf.d/
 
wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz 
wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz
 
gzip -d GeoIP.dat.gz 
gzip -d GeoLiteCity.dat.gz

echo "[mariadb]"  &gt;&gt;  /etc/yum.repos.d/MariaDB.repo
echo "name = MariaDB"  &gt;&gt;  /etc/yum.repos.d/MariaDB.repo
echo "baseurl = http://yum.mariadb.org/10.1/centos7-amd64"  &gt;&gt;  /etc/yum.repos.d/MariaDB.repo
echo "gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB"  &gt;&gt;  /etc/yum.repos.d/MariaDB.repo
echo "gpgcheck=1"  &gt;&gt;  /etc/yum.repos.d/MariaDB.repo

yum -y update 
yum -y install mariadb mariadb-server
systemctl enable mariadb
systemctl start mariadb
mysql_secure_installation</pre>