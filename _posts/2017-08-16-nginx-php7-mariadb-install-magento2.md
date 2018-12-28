---
ID: 972
post_title: >
  HTTP2.0
  配置测试使用，Nginx、PHP7和MariaDB安装
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/08/16/nginx-php7-mariadb-install-magento2/
published: true
post_date: 2017-08-16 14:53:19
---
<div class="postContent">
<div class="detail-content ">

配置HTTP2.0需要的是NGINX1.9.5以上的版本，以及一个HTTPS证书。这里我讲同时介绍HTTP2.0的LNMP环境搭建，所以同时需要用到PHP7以及MariaDB。

<hr />

<strong>NGINX安装配置</strong>

<hr />

首先到NGINX官网下载高于1.9.5的版本，<a href="http://nginx.org/%EF%BC%8C%E6%88%91%E8%BF%99%E9%87%8C%E6%8E%A8%E8%8D%90%E4%BD%BF%E7%94%A8%E7%A8%B3%E5%AE%9A%E7%9A%841.10.0" target="_blank" rel="nofollow noopener noreferrer">http://nginx.org/，我这里推荐使用稳定的1.10.0</a>。
cd 到 root目录下，下载nginx1.10.0
<blockquote>wget <a href="http://nginx.org/download/nginx-1.10.0.tar.gz" target="_blank" rel="nofollow noopener noreferrer">http://nginx.org/download/nginx-1.10.0.tar.gz</a></blockquote>
下载好后解压
<blockquote>tar -xvzf nginx-1.10.0.tar.gz</blockquote>
转到NGINX目录
<blockquote>cd nginx-1.10.0.tar.gz</blockquote>
准备编译环境
<blockquote>yum install gcc gcc-c++ autoconf automake zlib zlib-devel pcre-devel</blockquote>
需要注意的是HTTP2.0模块需要openssl1.0.1以上的版本来编译才能使用，而多数Linux/Unix系统下普遍为openssl1.0.1，所以需要下载openssl1.0.1以上的版本，配置中我使用的是openssl1.0.2h
<blockquote>wget <a href="https://www.openssl.org/source/openssl-1.0.2h.tar.gz" target="_blank" rel="nofollow noopener noreferrer">https://www.openssl.org/source/openssl-1.0.2h.tar.gz</a></blockquote>
同样解压
<blockquote>tar -xvzf openssl-1.0.2h.tar.gz</blockquote>
转入NGINX目录
<blockquote>cd nginx-1.10.0</blockquote>
检测NGINX编译环境
<pre class="prettyprint"><code><span class="pun">.</span><span class="str">/configure \
--prefix=/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx \
</span><span class="pun">--</span><span class="pln">sbin</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/usr/</span><span class="pln">sbin</span><span class="pun">/</span><span class="pln">nginx \
</span><span class="pun">--</span><span class="pln">conf</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/etc/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">.</span><span class="pln">conf \
</span><span class="pun">--</span><span class="pln">error</span><span class="pun">-</span><span class="pln">log</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">log</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">error</span><span class="pun">.</span><span class="pln">log \
</span><span class="pun">--</span><span class="pln">http</span><span class="pun">-</span><span class="pln">log</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">log</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">access</span><span class="pun">.</span><span class="pln">log \
</span><span class="pun">--</span><span class="pln">pid</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">.</span><span class="pln">pid \
</span><span class="pun">--</span><span class="kwd">lock</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">.</span><span class="kwd">lock</span><span class="pln"> \
</span><span class="pun">--</span><span class="pln">http</span><span class="pun">-</span><span class="pln">client</span><span class="pun">-</span><span class="pln">body</span><span class="pun">-</span><span class="pln">temp</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">cache</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">client_temp \
</span><span class="pun">--</span><span class="pln">http</span><span class="pun">-</span><span class="pln">proxy</span><span class="pun">-</span><span class="pln">temp</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">cache</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">proxy_temp \
</span><span class="pun">--</span><span class="pln">http</span><span class="pun">-</span><span class="pln">fastcgi</span><span class="pun">-</span><span class="pln">temp</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">cache</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">fastcgi_temp \
</span><span class="pun">--</span><span class="pln">http</span><span class="pun">-</span><span class="pln">uwsgi</span><span class="pun">-</span><span class="pln">temp</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">cache</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">uwsgi_temp \
</span><span class="pun">--</span><span class="pln">http</span><span class="pun">-</span><span class="pln">scgi</span><span class="pun">-</span><span class="pln">temp</span><span class="pun">-</span><span class="pln">path</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">cache</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">scgi_temp \
</span><span class="pun">--</span><span class="pln">user</span><span class="pun">=</span><span class="pln">nginx \
</span><span class="pun">--</span><span class="kwd">group</span><span class="pun">=</span><span class="pln">nginx \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">openssl</span><span class="pun">=</span><span class="str">/root/</span><span class="pln">openssl</span><span class="pun">-</span><span class="lit">1.0</span><span class="pun">.</span><span class="lit">2h</span><span class="pln"> \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_ssl_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_realip_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_addition_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_sub_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_dav_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_flv_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_mp4_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_gunzip_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_gzip_static_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_random_index_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_secure_link_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_stub_status_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_auth_request_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">mail \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">debug \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">mail_ssl_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">file</span><span class="pun">-</span><span class="pln">aio \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">ipv6 \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">threads \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">stream \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">stream_ssl_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_slice_module \
</span><span class="pun">--</span><span class="kwd">with</span><span class="pun">-</span><span class="pln">http_v2_module</span></code></pre>
成功后编译NGINX
<blockquote>make &amp;&amp; make install</blockquote>
编译完成后开始配置NGINX，首先修改/etc/nginx/nginx.conf
<pre class="prettyprint"><code><span class="pln">user  nginx nginx</span><span class="pun">;</span><span class="pln">
worker_processes  </span><span class="kwd">auto</span><span class="pun">;</span><span class="pln">
error_log           </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">error</span><span class="pun">.</span><span class="pln">log  info</span><span class="pun">;</span><span class="pln">
pid                 </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">.</span><span class="pln">pid</span><span class="pun">;</span><span class="pln">
lock_file           </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">.</span><span class="kwd">lock</span><span class="pun">;</span><span class="pln">
events </span><span class="pun">{</span><span class="pln">
    worker_connections      </span><span class="lit">4096</span><span class="pun">;</span><span class="pln">
    accept_mutex            off</span><span class="pun">;</span><span class="pun">}</span><span class="pln">
http </span><span class="pun">{</span><span class="pln">
    include       mime</span><span class="pun">.</span><span class="pln">types</span><span class="pun">;</span><span class="pln">
    server_names_hash_bucket_size </span><span class="lit">64</span><span class="pun">;</span><span class="pln"> 
    default_type  application</span><span class="pun">/</span><span class="pln">octet</span><span class="pun">-</span><span class="pln">stream</span><span class="pun">;</span><span class="pln">
    access_log              off</span><span class="pun">;</span><span class="pln">
    aio                     threads</span><span class="pun">;</span><span class="pln">
    sendfile                on</span><span class="pun">;</span><span class="pln">
    sendfile_max_chunk      </span><span class="lit">512k</span><span class="pun">;</span><span class="pln">
    tcp_nopush              on</span><span class="pun">;</span><span class="pln">
    tcp_nodelay             on</span><span class="pun">;</span><span class="pln">
    keepalive_timeout       </span><span class="lit">5</span><span class="pun">;</span><span class="pln">
    gzip                    on</span><span class="pun">;</span><span class="pln">
    gzip_disable            </span><span class="pun">“</span><span class="pln">MSIE </span><span class="pun">[</span><span class="lit">1</span><span class="pun">-</span><span class="lit">6</span><span class="pun">].(?!.*</span><span class="pln">SV1</span><span class="pun">)”;</span><span class="pln">
    gzip_http_version       </span><span class="lit">1.1</span><span class="pun">;</span><span class="pln">
    gzip_vary               on</span><span class="pun">;</span><span class="pln">
    gzip_proxied            any</span><span class="pun">;</span><span class="pln">
    gzip_min_length         </span><span class="lit">1000</span><span class="pun">;</span><span class="pln">
    gzip_buffers            </span><span class="lit">16</span><span class="lit">8k</span><span class="pun">;</span><span class="pln">
    gzip_comp_level         </span><span class="lit">6</span><span class="pun">;</span><span class="pln">
    client_max_body_size    </span><span class="lit">256m</span><span class="pun">;</span><span class="pln">
    gzip_types              text</span><span class="pun">/</span><span class="pln">plain text</span><span class="pun">/</span><span class="pln">css text</span><span class="pun">/</span><span class="pln">xml text</span><span class="pun">/</span><span class="pln">javascript application</span><span class="pun">/</span><span class="pln">json application</span><span class="pun">/</span><span class="pln">x</span><span class="pun">-</span><span class="pln">javascript application</span><span class="pun">/</span><span class="pln">xml application</span><span class="pun">/</span><span class="pln">xml</span><span class="pun">+</span><span class="pln">rss</span><span class="pun">;</span><span class="pln">
    log_format  main        </span><span class="str">'$remote_addr - $remote_user [$time_local] "$request" '</span><span class="str">'$status $body_bytes_sent "$http_referer" '</span><span class="str">'"$http_user_agent" "$http_x_forwarded_for"'</span><span class="pun">;</span><span class="pln">
    proxy_connect_timeout </span><span class="lit">5</span><span class="pun">;</span><span class="pln">
    proxy_read_timeout </span><span class="lit">60</span><span class="pun">;</span><span class="pln">
    proxy_send_timeout </span><span class="lit">5</span><span class="pun">;</span><span class="pln">
    proxy_buffer_size </span><span class="lit">16k</span><span class="pun">;</span><span class="pln">
    proxy_buffers </span><span class="lit">4</span><span class="lit">64k</span><span class="pun">;</span><span class="pln">
    proxy_busy_buffers_size </span><span class="lit">128k</span><span class="pun">;</span><span class="pln">
    proxy_temp_file_write_size </span><span class="lit">128k</span><span class="pun">;</span><span class="pln">
    proxy_temp_path </span><span class="pun">/</span><span class="pln">home</span><span class="pun">/</span><span class="pln">temp_dir</span><span class="pun">;</span><span class="pln">
    include vhost</span><span class="com">/*.conf;
}</span></code></pre>
然后在/etc/nginx/vhost中添加个虚拟主机目录，比如www.XXX.com.conf，这里的证书可以在沃通中免费申请，红色是配置SSL模块和H2模块的重要部分
<pre class="prettyprint"><code><span class="pln">server </span><span class="pun">{</span><span class="pln">
    listen       </span><span class="lit">443</span><span class="pln"> ssl http2</span><span class="pun">;</span><span class="pln">
    listen       </span><span class="pun">[::]:</span><span class="lit">443</span><span class="pln"> ssl http2</span><span class="pun">;</span><span class="pln">
    server_name  www</span><span class="pun">.</span><span class="pln">XXX</span><span class="pun">.</span><span class="pln">com XXX</span><span class="pun">.</span><span class="pln">com</span><span class="pun">;</span><span class="pln">
    charset             utf</span><span class="pun">-</span><span class="lit">8</span><span class="pun">;</span><span class="pln">
    ssl_certificate      </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">vhost</span><span class="pun">/</span><span class="pln">www</span><span class="pun">.</span><span class="pln">XXX</span><span class="pun">.</span><span class="pln">com</span><span class="pun">.</span><span class="pln">crt</span><span class="pun">;</span><span class="pln">
    ssl_certificate_key  </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">vhost</span><span class="pun">/</span><span class="pln">www</span><span class="pun">.</span><span class="pln">XXX</span><span class="pun">.</span><span class="pln">com</span><span class="pun">.</span><span class="pln">key</span><span class="pun">;</span><span class="pln">
    ssl_session_cache shared</span><span class="pun">:</span><span class="pln">SSL</span><span class="pun">:</span><span class="lit">20m</span><span class="pun">;</span><span class="pln">
    ssl_session_timeout </span><span class="lit">10m</span><span class="pun">;</span><span class="pln">
    ssl_protocols </span><span class="typ">TLSv1</span><span class="typ">TLSv1</span><span class="pun">.</span><span class="lit">1</span><span class="typ">TLSv1</span><span class="pun">.</span><span class="lit">2</span><span class="pun">;</span><span class="pln">
    ssl_prefer_server_ciphers on</span><span class="pun">;</span><span class="pln">
    ssl_ciphers </span><span class="str">'ECDH+AESGCM:ECDH+AES256:ECDH+AES128:DH+3DES:!ADH:!AECDH:!MD5'</span><span class="pun">;</span><span class="pln">
    root     </span><span class="pun">/</span><span class="pln">home</span><span class="pun">/</span><span class="pln">www</span><span class="pun">.</span><span class="pln">XXX</span><span class="pun">.</span><span class="pln">com</span><span class="pun">;</span><span class="pln">
    location </span><span class="pun">/</span><span class="pun">{</span><span class="pln">
        aio    threads</span><span class="pun">=</span><span class="kwd">default</span><span class="pun">;</span><span class="pln">
        index  index</span><span class="pun">.</span><span class="pln">html index</span><span class="pun">.</span><span class="pln">htm index</span><span class="pun">.</span><span class="pln">php</span><span class="pun">;</span><span class="pun">}</span><span class="pln">
    error_page  </span><span class="lit">404</span><span class="lit">403</span><span class="pun">/</span><span class="lit">404.html</span><span class="pun">;</span><span class="pln">
    error_page  </span><span class="lit">500</span><span class="lit">502</span><span class="lit">503</span><span class="lit">504</span><span class="pun">/</span><span class="lit">50x</span><span class="pun">.</span><span class="pln">html</span><span class="pun">;</span><span class="pln">
    location </span><span class="pun">~</span><span class="pln"> \.php$ </span><span class="pun">{</span><span class="pln">
        fastcgi_pass                    </span><span class="lit">127.0</span><span class="pun">.</span><span class="lit">0.1</span><span class="pun">:</span><span class="lit">9000</span><span class="pun">;</span><span class="pln">
        fastcgi_index                   index</span><span class="pun">.</span><span class="pln">php</span><span class="pun">;</span><span class="pln">
        fastcgi_split_path_info         </span><span class="pun">^((?</span><span class="pln">U</span><span class="pun">).+</span><span class="pln">\.php</span><span class="pun">)(/?.+)</span><span class="pln">$</span><span class="pun">;</span><span class="pln">
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name</span><span class="pun">;</span><span class="pln">
        include                         fastcgi_params</span><span class="pun">;</span><span class="pun">}</span><span class="pun">}</span></code></pre>
配置完成后在给Linux添加个NGINX用户和用户组
<blockquote>groupadd -r nginx
useradd -s /sbin/nologin -g nginx -r nginx</blockquote>
最后检测NGINX配置是否正常
<blockquote>nginx -t</blockquote>
正常的情况下加入Systemctl模块，开机启动，到/usr/lib/systemd/system目录下创建nginx.service
<pre class="prettyprint"><code><span class="pun">[</span><span class="typ">Unit</span><span class="pun">]</span><span class="typ">Description</span><span class="pun">=</span><span class="pln">nginx </span><span class="pun">-</span><span class="pln"> high performance web server 
</span><span class="typ">Documentation</span><span class="pun">=</span><span class="pln">http</span><span class="pun">:</span><span class="com">//nginx.org/en/docs/</span><span class="typ">After</span><span class="pun">=</span><span class="pln">network</span><span class="pun">.</span><span class="pln">target remote</span><span class="pun">-</span><span class="pln">fs</span><span class="pun">.</span><span class="pln">target nss</span><span class="pun">-</span><span class="pln">lookup</span><span class="pun">.</span><span class="pln">target
</span><span class="pun">[</span><span class="typ">Service</span><span class="pun">]</span><span class="typ">Type</span><span class="pun">=</span><span class="pln">forking
</span><span class="typ">PIDFile</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">.</span><span class="pln">pid
</span><span class="typ">ExecStartPre</span><span class="pun">=</span><span class="str">/usr/</span><span class="pln">sbin</span><span class="pun">/</span><span class="pln">nginx </span><span class="pun">-</span><span class="pln">t </span><span class="pun">-</span><span class="pln">c </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">.</span><span class="pln">conf
</span><span class="typ">ExecStart</span><span class="pun">=</span><span class="str">/usr/</span><span class="pln">sbin</span><span class="pun">/</span><span class="pln">nginx </span><span class="pun">-</span><span class="pln">c </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">.</span><span class="pln">conf
</span><span class="typ">ExecReload</span><span class="pun">=</span><span class="str">/bin/</span><span class="pln">kill </span><span class="pun">-</span><span class="pln">s HUP $MAINPID
</span><span class="typ">ExecStop</span><span class="pun">=</span><span class="str">/bin/</span><span class="pln">kill </span><span class="pun">-</span><span class="pln">s QUIT $MAINPID
</span><span class="typ">PrivateTmp</span><span class="pun">=</span><span class="kwd">true</span><span class="pun">[</span><span class="typ">Install</span><span class="pun">]</span><span class="typ">WantedBy</span><span class="pun">=</span><span class="pln">multi</span><span class="pun">-</span><span class="pln">user</span><span class="pun">.</span><span class="pln">target</span></code></pre>
保存后，加入开机启动
<blockquote>systemctl enable nginx</blockquote>
开启nginx
<blockquote>systemctl start nginx</blockquote>
验证HTTP2.0是否成功可以使用Chrome的开发模式下的NetWork，如图
<img title="HTTP2.0配置测试使用_" src="http://hss5.com/wp-content/uploads/2017/08/575e757c000148b707950645.png" alt='图片描述' />
如果这里显示的是H2则说明配置成功！！

<hr />

<strong>PHP-FPM安装配置</strong>

<hr />

同样首先到php官网下载php.tar.gz，推荐使用最新的PHP7.0.6，将他上传到root中，解压
<blockquote>tar php.tar.gz
cd php</blockquote>
安装PHP所需环境
<blockquote>yum install libacl libacl-devel libxml2 libxml2-devel openssl openssl-devel bzip2 bzip2-devel libcurl libcurl-devel enchant enchant-devel gd gd-devel gmp gmp-devel libmcrypt libmcrypt-devel libtidy libtidy-devel libxslt libxslt-devel</blockquote>
检测编译
<blockquote>'./configure' '--disable-debug' '--disable-rpath' '--enable-fpm' '--with-fpm-user=nginx' '--with-fpm-group=nginx' '--with-fpm-acl' '--with-libxml-dir' '--with-openssl' '--with-kerberos' '--with-pcre-regex' '--with-zlib' '--enable-bcmath' '--with-bz2' '--enable-calendar' '--with-curl' '--enable-dba' '--with-enchant' '--enable-exif' '--disable-fileinfo' '--with-pcre-dir' '--enable-ftp' '--with-gd' '--with-jpeg-dir' '--with-png-dir' '--with-zlib-dir' '--with-xpm-dir' '--with-freetype-dir' '--enable-gd-native-ttf' '--with-gettext' '--with-gmp' '--with-mhash' '--enable-mbstring' '--enable-mbregex' '--with-mcrypt' '--with-mysqli' '--enable-embedded-mysqli' '--with-mysql-sock=/tmp/mysql.sock' '--enable-pcntl' '--with-pdo-mysql' '--enable-session' '--enable-shmop' '--enable-soap' '--enable-sockets' '--enable-sysvsem' '--with-tidy' '--enable-wddx' '--with-xmlrpc' '--enable-xml' '--with-iconv-dir' '--with-xsl' '--enable-zip' '--enable-mysqlnd' '--without-pear' '--enable-shared'</blockquote>
成功后编译
<blockquote>make &amp;&amp; make install</blockquote>
编译完成后配置php-fpm，到/usr/local/etc修改php-fpm.conf
将最底部的修改为include=/usr/local/etc/php-fpm.d/*.conf，然后修改/usr/local/etc/php-fpm.d/www.conf，修改两处
<blockquote>;pool name ('www' here)
[nginx]
user = nginx
group = nginx</blockquote>
修改完成后检测是否正确
<blockquote>php-fpm -t</blockquote>
正确添加Systemctl模块，直接到php.tar.gz解压后的文件夹中找/sapi/fpm/init.d.php-fpm，并把他复制到/etc/init.d/中，添加权限
<blockquote>chmod +x /etc/init.d/php-fpm
chkconfig --add php-fpm
chkconfig php-fpm on</blockquote>
加入开机启动
<blockquote>systemctl enable php-fpm</blockquote>
开启php-fpm
<blockquote>systemctl start php-fpm</blockquote>
创建phpinfo测试一下，如图
<img title="HTTP2.0配置测试使用_" src="http://hss5.com/wp-content/uploads/2017/08/575e766000015d1a19200952.png" alt='图片描述' />

<hr />

<strong>MaraiDB安装配置</strong>

<hr />

MariaDB的安装可以直接添加更新官方的源，到/etc/yum.repos.d下创建MariaDB.repo
<blockquote>[mariadb]
name = MariaDB
baseurl = <a href="http://yum.mariadb.org/10.1/centos7-amd64" target="_blank" rel="nofollow noopener noreferrer">http://yum.mariadb.org/10.1/centos7-amd64</a>
gpgkey=<a href="https://yum.mariadb.org/RPM-GPG-KEY-MariaDB" target="_blank" rel="nofollow noopener noreferrer">https://yum.mariadb.org/RPM-GPG-KEY-MariaDB</a>
gpgcheck=1</blockquote>
保存后更新源
<blockquote>yum update</blockquote>
安装最新MariaDB
<blockquote>yum install mariadb mariadb-server</blockquote>
添加开机启动
<blockquote>systemctl enable mariadb</blockquote>
开启
<blockquote>systemctl start mariadb</blockquote>
在服务开启的状态下，设置最新密码
<blockquote>mysql_secure_installation</blockquote>
</div>
</div>