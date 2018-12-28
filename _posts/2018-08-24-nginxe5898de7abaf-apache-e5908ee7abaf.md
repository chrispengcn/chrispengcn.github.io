---
ID: 1304
post_title: nginx前端 apache 后端
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/08/24/nginx%e5%89%8d%e7%ab%af-apache-%e5%90%8e%e7%ab%af/'
published: true
post_date: 2018-08-24 18:10:23
---
<h1 class="postTitle"><a id="cb_post_title_url" class="postTitle2" href="https://www.cnblogs.com/yanghaiyan/p/7097393.html">可以同时装apache和nginx么,可以! 传闻中的反向代理</a></h1>
<div class="clear"></div>
<div class="postBody">
<div id="cnblogs_post_body" class="blogpost-body">
<div>1》  <a href="https://zhidao.baidu.com/question/1431111405798861259.html">https://zhidao.baidu.com/question/1431111405798861259.html</a></div>
<div>
<div>可以同时装apache和nginx么</div>
</div>
<div>可以，在linux下，有的会用apache去跑php，然后用nginx做反向代理，比如apache运行在8080端口，nginx在80端口，访问php文件时，反向代理到apache，静态页通过nginx处理。nginx支持高并发，apache对php的运行比较稳定。</div>
<div></div>
<div>2》  <a href="http://blog.csdn.net/ITYang_/article/details/53907937">http://blog.csdn.net/ITYang_/article/details/53907937</a></div>
<div>ECS是英文词组Elastic Compute Service的缩写，意思是弹性计算服务器</div>
<div>
<h3>同一个端口是不能同时有两个程序监听的。所以换个思路解决同一台服务器下某些网站运行在nginx下，某些网站运行在Apache下共存。</h3>

<hr />

<h4>解决思路：</h4>
<blockquote>将nginx作为代理服务器和web服务器使用，nginx监听80端口，Apache监听除80以外的端口，我这暂时使用8080端口。</blockquote>
<img src="https://images2015.cnblogs.com/blog/1069155/201707/1069155-20170702091441789-255935999.png" alt="" />

&nbsp;
<h4>解决方案：</h4>
<ul>
 	<li>在Linux 一经搭建好环境 先后安装了Nginx 和Apache 由于 默认端口都是：80一般客户请求的服务器端口默认为80 所以Nginx作为静态页端口设置：80;Apache设置端口为:8080(在httpd.conf 文件中修改Listen：8080)Apache下的网站：</li>
</ul>
在nginx.conf中 添加
<pre><code>　　server {
            listen       80;
            server_name  www.one.ityangs.cn one.ityangs.cn;

location / {            
            proxy_pass              http://127.0.0.1:8080;              
            proxy_redirect          off;        
            proxy_set_header Host $host;       
            proxy_set_header X-Real-IP $remote_addr;       
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;           
            } 
}</code></pre>
<ul>
 	<li>123456789101112</li>
</ul>
在httpd.conf中 添加 (/etc/httpd/conf/httpd.conf)
<pre><code>&lt;virtualhost *:8080&gt;
ServerName  <a href="http://www.one.ityangs.cn/">www.one.ityangs.cn</a>
ServerAlias  <a href="http://www.one.ityangs.cn/">www.one.ityangs.cn</a> <a href="http://one.ityangs.cn/">one.ityangs.cn</a> 
DocumentRoot  /www/one
DirectoryIndex index.php index.html
&lt;Directory /www/one&gt;
Options +Includes +FollowSymLinks -Indexes
AllowOverride All
Order Deny,Allow
Allow from All
&lt;/Directory&gt;
&lt;/virtualhost&gt;</code></pre>
<ul>
 	<li>123456789101112</li>
</ul>
<ul>
 	<li>Nginx下的网站：</li>
</ul>
在nginx.conf中 添加
<div><code><code> server {
listen       80;
server_name    two.ityangs.cn www.two.ityangs.cn;
root   /www/two;
location /{</code></code>index  index.html index.htm index.php;
if (!-e $request_filename) {
rewrite  ^(.*)$  /index.php?s=$1  last;
break;
}
error_page 404  /var/www/html/404.html;
}

<code><code></code></code>location ~ \.php(.*)$ {
fastcgi_pass   127.0.0.1:9000;
fastcgi_index  index.php;
fastcgi_split_path_info  ^((?U).+\.php)(/?.+)$;
fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
fastcgi_param  PATH_INFO  $fastcgi_path_info;
fastcgi_param  PATH_TRANSLATED  $document_root$fastcgi_path_info;
include        fastcgi_params;
}

<code><code></code></code>}

</div>
</div>
<div></div>
</div>
</div>