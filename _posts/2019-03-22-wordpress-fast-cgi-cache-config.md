---
ID: 2645
post_title: >
  教你如何利用fastcgi_cache缓存加速WordPress
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/03/22/wordpress-fast-cgi-cache-config/
published: true
post_date: 2019-03-22 19:44:51
---
<div class="table-box">
<table>
<tbody>
<tr>
<td><strong>WordPress有很多的缓存加速方案，例如插件缓存（wp-super-cache、wp-rocket等）、PHP代码缓存等等，现分享本站使用的<a href="https://www.baidu.com/s?wd=nginx&amp;tn=24004469_oem_dg&amp;rsv_dl=gh_pl_sl_csd" target="_blank" rel="noopener">nginx</a>缓存。利用fastcgi_cache缓存。
</strong></td>
</tr>
</tbody>
</table>
</div>
在使用nginx缓存之前，必须在nginx里面加载专门的模块，这个模块叫做ngx_cache_purge。
<div><strong>添加ngx_cache_purge模块</strong></div>
<div><strong>下载ngx_cache_purge模块</strong></div>
ngx_cache_purge模块的官方地址：http://labs.frickle.com/files/。在这个地址找到最新版的模块版本 ，使用wget下载。
<pre>wget http://labs.frickle.com/files/ngx_cache_purge-2.3.tar.gz
tar zxvf ngx_cache_purge-2.3.tar.gz</pre>
我这里使用的就是ngx_cache_purge-2.3。
<div><strong>编译安装ngx_cache_purge模块</strong></div>
使用nginx -V命令查看nginx是否已经安装了这个模块，如果没有安装，需要重新编译安装。

使用军哥lnmp一键安装包的同学，可以在lnmp的安装目录中找到lnmp.conf这个文件，然后在nginx模块中添加ngx_cache_purge。之后重新平滑升级nginx即可。
<div><strong>修改ngxin配置</strong></div>
在使用fastcgi_cache缓存之前，必须先修改nginx配置，具体就是进入虚拟主机配置中，找到domainname.conf，然后修改里面的sever配置。
<pre>#下面2行的中的wpcache路径请自行提前创建，否则可能会路径不存在而无法启动nginx，max_size请根据分区大小自行设置
fastcgi_cache_path /tmp/wpcache levels=1:2 keys_zone=WORDPRESS:250m inactive=1d max_size=1G;
fastcgi_temp_path /tmp/wpcache/temp;
fastcgi_cache_key "$scheme$request_method$host$request_uri";
fastcgi_cache_use_stale error timeout invalid_header http_500;
#忽略一切nocache申明，避免不缓存伪静态等
fastcgi_ignore_headers Cache-Control Expires Set-Cookie;
#Ps：如果是多个站点，以上内容不要重复添加，否则会冲突，可以考虑将以上内容添加到nginx.conf里面，避免加了多次。
server
    {
        listen 80;
        #请修改为自己的域名
        server_name zhangge.net;
        index index.html index.htm index.php default.html default.htm default.php;
        #请修改为自己网站的存放路径
        root  /home/wwwroot/domainname.com;
        set $skip_cache 0;
        #post访问不缓存
        if ($request_method = POST) {
            set $skip_cache 1;
        }
        #动态查询不缓存
        if ($query_string != "") {
            set $skip_cache 1;
        }
        #后台等特定页面不缓存（其他需求请自行添加即可）
        if ($request_uri ~* "/wp-admin/|/xmlrpc.php|wp-.*.php|/feed/|index.php|sitemap(_index)?.xml") {
            set $skip_cache 1;
        }
        #对登录用户、评论过的用户不展示缓存（这个规则张戈博客并没有使用，所有人看到的都是缓存）
        if ($http_cookie ~* "comment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_no_cache|wordpress_logged_in") {
            set $skip_cache 1;
        }
        #这里请参考你网站之前的配置，特别是sock的路径，弄错了就502了！
        location ~ [^/]\.php(/|$)
            {
                try_files $uri =404;
                fastcgi_pass  unix:/tmp/php-cgi.sock;
                fastcgi_index index.php;
                include fastcgi.conf;
                #新增的缓存规则
                fastcgi_cache_bypass $skip_cache;
                fastcgi_no_cache $skip_cache;
                add_header X-Cache "$upstream_cache_status From $host";
                fastcgi_cache WORDPRESS;
                fastcgi_cache_valid 200 301 302 1d;
        }
        location / {
                #此处可以添加自定义的伪静态规则（之前你新增的伪静态规则可以添加到这，没有就不用了）
                try_files $uri $uri/ /index.php?$args;
                rewrite /wp-admin$ $scheme://$host$uri/ permanent;
         }
        #<a href="https://www.baidu.com/s?wd=%E7%BC%93%E5%AD%98%E6%B8%85%E7%90%86&amp;tn=24004469_oem_dg&amp;rsv_dl=gh_pl_sl_csd" target="_blank" rel="noopener">缓存清理</a>配置（可选模块，请细看下文说明）
        location ~ /purge(/.*) {
            allow 127.0.0.1;
            allow "此处填写你<a href="https://www.baidu.com/s?wd=%E6%9C%8D%E5%8A%A1%E5%99%A8&amp;tn=24004469_oem_dg&amp;rsv_dl=gh_pl_sl_csd" target="_blank" rel="noopener">服务器</a>的真实外网IP";
            deny all;
            fastcgi_cache_purge WORDPRESS "$scheme$request_method$host$1";
        }
        location ~* ^.+\.(ogg|ogv|svg|svgz|eot|otf|woff|mp4|ttf|rss|atom|jpg|jpeg|gif|png|ico|zip|tgz|gz|rar|bz2|doc|xls|exe|ppt|tar|mid|midi|wav|bmp|rtf)$ {
                access_log off; log_not_found off; expires max;
        }
        location = /robots.txt { access_log off; log_not_found off; }
        location ~ /\. { deny  all; access_log off; log_not_found off; }
        #请注意修改日志路径
        access_log /home/wwwlogs/domainname.com.log access;</pre>
注意修改上述代码中的该修改部分，不然nginx重启会出错。当然，如果是启用了https，模块就应相应的改变。
<div><strong>安装Nginx-helper插件</strong></div>
在后台搜索nginx-helper，安装好插件。

关于插件的设置：
<div><strong>如果没有使用CDN就可以选择purge模式，如果使用了CDN最好选择文件模式。</strong></div>
由于插件作者定义的缓存路径是 /var/run/nginx-cache ，而我们可能会根据服务器实际情况来自定义缓存路径，这样一来，缓存路径的不同就会导致插件无法找到缓存文件并删除！

解决的方法：在wp-config.php中增加一行代码：
<pre>define( 'RT_WP_NGINX_HELPER_CACHE_PATH','/tmp/wpcache');</pre>
这样，就配置好了。