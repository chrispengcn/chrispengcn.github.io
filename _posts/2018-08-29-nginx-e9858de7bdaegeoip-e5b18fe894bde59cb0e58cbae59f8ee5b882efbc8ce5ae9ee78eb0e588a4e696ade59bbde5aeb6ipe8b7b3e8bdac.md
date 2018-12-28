---
ID: 1327
post_title: >
  nginx 配置geoip
  屏蔽地区城市，实现判断国家IP跳转
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/08/29/nginx-%e9%85%8d%e7%bd%aegeoip-%e5%b1%8f%e8%94%bd%e5%9c%b0%e5%8c%ba%e5%9f%8e%e5%b8%82%ef%bc%8c%e5%ae%9e%e7%8e%b0%e5%88%a4%e6%96%ad%e5%9b%bd%e5%ae%b6ip%e8%b7%b3%e8%bd%ac/'
published: true
post_date: 2018-08-29 17:59:18
---
<pre>步骤    ./configure  --with-http_stub_status_module --with-http_ssl_module --with-http_v2_module --with-http_gzip_static_module --with-ipv6 --with-http_sub_module --with-http_flv_module --user=www --group=www --with-http_gzip_static_module --with-http_geoip_module</pre>
&nbsp;

相应的省份代码：

CN,01,”Anhui”
CN,02,”Zhejiang”
CN,03,”Jiangxi”
CN,04,”Jiangsu”
CN,05,”Jilin”
CN,06,”Qinghai”
CN,07,”Fujian”
CN,08,”Heilongjiang”
CN,09,”Henan”
CN,10,”Hebei”
CN,11,”Hunan”
CN,12,”Hubei”
CN,13,”Xinjiang”
CN,14,”Xizang”
CN,15,”Gansu”
CN,16,”Guangxi”
CN,18,”Guizhou”
CN,19,”Liaoning”
CN,20,”Nei Mongol”
CN,21,”Ningxia”
CN,22,”Beijing”
CN,23,”Shanghai”
CN,24,”Shanxi”
CN,25,”Shandong”
CN,26,”Shaanxi”
CN,28,”Tianjin”
CN,29,”Yunnan”
CN,30,”Guangdong”
CN,31,”Hainan”
CN,32,”Sichuan”
CN,33,”Chongqing”

&nbsp;

安装 Nginx
因为要用到 http_geoip_module 模块，系统自带的 nginx 一般不带这个模块，所以要下载 nginx 源代码后自行编译：
<pre># wget <a href="http://nginx.org/download/nginx-0.9.6.tar.gz">http://nginx.org/download/nginx-0.9.6.tar.gz</a>
 # tar zxvf nginx-0.9.6.tar.gz
 # cd nginx-0.9.6
 # ./configure --without-http_empty_gif_module --with-poll_module \
 --with-http_stub_status_module --with-http_ssl_module \
 --with-http_geoip_module
 # make; make install</pre>
安装 MaxMind 的 GeoIP 库
MaxMind 提供了免费的 IP 地域数据库（GeoIP.dat），不过这个数据库文件是二进制的，需要用 GeoIP 库来读取，所以除了要下载 GeoIP.dat 文件外（见下一步），还需要安装能读取这个文件的库。
<pre># wget <a href="http://geolite.maxmind.com/download/geoip/api/c/GeoIP.tar.gz">http://geolite.maxmind.com/download/geoip/api/c/GeoIP.tar.gz</a>
 # tar -zxvf GeoIP.tar.gz
 # cd GeoIP-1.4.6
 # ./configure
 # make; make install



</pre>
刚才安装的库自动安装到 /usr/local/lib 下，所以这个目录需要加到动态链接配置里面以便运行相关程序的时候能自动绑定到这个 GeoIP 库：
<pre># echo '/usr/local/lib' &gt; /etc/ld.so.conf.d/geoip.conf
 # ldconfig</pre>
下载 IP 数据库
MaxMind 提供了免费的 IP 地域数据库，这个数据库是二进制的，不能用文本编辑器打开，需要上面的 GeoIP 库来读取：

# wget <a href="http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz">http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz</a>
# gunzip GeoIP.dat.gz

配置 Nginx
最后是配置 nginx，在相关地方加上如下的配置就可以了：
<pre># vi /etc/nginx/nginx.conf
 ...
 geoip_country /home/vpsee/GeoIP.dat;

fastcgi_param GEOIP_COUNTRY_CODE $geoip_country_code;
 fastcgi_param GEOIP_COUNTRY_CODE3 $geoip_country_code3;
 fastcgi_param GEOIP_COUNTRY_NAME $geoip_country_name;
 ...

if ($geoip_country_code = CN) {
 root /home/vpsee/cn/;
 }</pre>
这样，当来自中国的 IP 访问网站后就自动访问到预定的 /home/vpsee/cn 页面。关于 Nginx + GeoIP 还有很多有用的用法，比如做个简单的 CDN，来自中国的访问自动解析到国内服务器、来自美国的访问自动转向到美国服务器等。MaxMind 还提供了全球各个城市的 IP 信息，还可以下载城市 IP 数据库来针对不同城市做处理。