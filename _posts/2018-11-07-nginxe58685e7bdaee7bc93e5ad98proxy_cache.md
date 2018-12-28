---
ID: 1421
post_title: nginx内置缓存Proxy_cache
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/07/nginx%e5%86%85%e7%bd%ae%e7%bc%93%e5%ad%98proxy_cache/'
published: true
post_date: 2018-11-07 11:38:32
---
proxy_cache作用是缓存后端服务器的内容

一．http模块加入缓存设置
1.加上下面三段（缓存到磁盘）
<pre>proxy_temp_path /etc/nginx/proxy_temp;
proxy_cache_path /etc/nginx/proxy_cache levels=1:2 keys_zone=content:20m inactive=1d max_size=100m;
proxy_ignore_headers X-Accel-Expires Expires Cache-Control Set-Cookie;

proxy_ignore_headers 不处理后端服务器返回的指定响应头</pre>
2.也可以缓存到内存（/dev/shm）,提高访问速度，但重启失效，需要重新mount
<pre>mkdir /dev/shm/proxy_temp 
mkdir /dev/shm/proxy_cache

chmod -R 777 /dev/shm/proxy*

mkdir /mnt/nginx_temp /mnt/nginx_cache

mount –bind /dev/shm/proxy_temp /mnt/nginx_temp 
mount –bind /dev/shm/proxy_cache /mnt/nginx_cache</pre>
–bind 挂载目录会集成之前的权限和所属

上面三段修改为：
<pre>proxy_temp_path /mnt/nginx_temp;
proxy_cache_path /mnt/nginx_cache levels=1:2 keys_zone=cache_one:200m inactive=5d max_size=400m;
proxy_ignore_headers X-Accel-Expires Expires Cache-Control Set-Cookie;

相关参数解释：</pre>
proxy_temp_path：缓存临时文件路径
proxy_cache_path：缓存路径 levels设置目录层次 keys_zone设置缓存名字和共享内存大小(设置一个共享内存区，该内存区用于存储缓存键和元数据，有些类似计时器的用途。将键的拷贝放入内存可以使 NGINX 在不检索磁盘的情况下快速决定一个请求是 HIT 还是 MISS ，这样大大提高了检索速度。一个 1MB 的内存空间可以存储大约 8000 个 key) inactive在指定时间内没人访问则被删除
max_size最大缓存空间
proxy_ignore_headers 禁止处理来自代理服务器的应答
<pre>二．Server模块
server {
 listen 80;
 server_name www.nginx1.com;

#charset koi8-r;

#access_log logs/host.access.log main
 #缓存静态文件,一旦固定就很少变化
 location ~ .*\.(html|htm|css|js|ico|jpeg|git|jpg|png|bmp|swf)$ {
 root /home/tyt/apache-tomcat-7.0.64/webapps/ylyg01;
 proxy_pass http://127.0.0.1:8080;
 proxy_cookie_path /test/ /;
 proxy_set_header Host $host;
 proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
 proxy_set_header X-Real-IP $remote_addr;

 proxy_cache content;
 proxy_cache_valid 200 304 301 302 10d;
 proxy_cache_valid any 1d;
 proxy_cache_key $host$uri$is_args$args;
 }
 #动态文件不缓存
 location ~ .*$ {
 proxy_set_header Host $host;
 proxy_set_header X-Real-IP $remote_addr;
 proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
 proxy_pass http://127.0.0.1:8080;
 }
}

相关参数解释：</pre>
proxy_cache content; 根keys_zone后的内容对应
proxy_cache_valid 200 304 301 302 10d; 哪些状态缓存多长时间
proxy_cache_valid any 1d; 其他的缓存多长时间
proxy_cache_key $host$uri$is_args$args; 通过key来hash，定义KEY的值

三．测试：
在配置的指定目录下（这里是/etc/nginx/proxy_cache，可发现缓存文件夹,这里是磁盘缓存的图）

其缓存的具体目录结构如下（缓存的文件名和key为代理URL的MD5 码）：

原文：https://blog.csdn.net/tshangshi/article/details/51884279