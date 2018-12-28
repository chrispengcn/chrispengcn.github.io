---
ID: 1686
post_title: Nginx跳转的配置实例
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/10/nginx%e8%b7%b3%e8%bd%ac%e7%9a%84%e9%85%8d%e7%bd%ae%e5%ae%9e%e4%be%8b/'
published: true
post_date: 2018-12-10 15:32:27
---
<div class="article-header-box">
<div class="article-header">
<div class="article-title-box"></div>
<div class="article-info-box">
<div class="operating">相信大家在日常运维工作中如果你用到nginx作为前端反向代理服务器的话，你会对nginx的rewrite又爱又恨，爱它是因为你搞定了它，完成了开发人员的跳转需求后你会觉得很爽，觉得真的很强大，恨它是因为当一些稀奇古怪跳转的需求时候你会没有头绪、百般调试、上网求神拜佛都搞不定的时候真是想死的心都有了，当然网上也有很多nginx rewrite的经典示例，但是我感觉对我的工作都没有太大的帮助</div>
</div>
</div>
</div>
<article class="baidu_pl">
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
<div id="content_views" class="htmledit_views">

下面是我工作中遇到的一些rewrite示例。提供给大家分享

正则表达式匹配，其中：
<ol class="hl-main ln-show list-paddingleft-2" title="Double click to hide line number.">
 	<li>* ~ 为区分大小写匹配</li>
 	<li>* ~* 为不区分大小写匹配</li>
 	<li>* !~和!~*分别为区分大小写不匹配及不区分大小写不匹配</li>
</ol>
文件及目录匹配，其中：
<ol class="hl-main ln-show list-paddingleft-2" title="Double click to hide line number.">
 	<li>* -f和!-f用来判断是否存在文件</li>
 	<li>* -d和!-d用来判断是否存在目录</li>
 	<li>* -e和!-e用来判断是否存在文件或目录</li>
 	<li>* -x和!-x用来判断文件是否可执行</li>
</ol>
flag标记有：
<ol class="hl-main ln-show list-paddingleft-2" title="Double click to hide line number.">
 	<li>* last 相当于Apache里的[L]标记，表示完成rewrite</li>
 	<li>* break 终止匹配, 不再匹配后面的规则</li>
 	<li>* redirect 返回302临时重定向 地址栏会显示跳转后的地址</li>
 	<li>* permanent 返回301永久重定向 地址栏会显示跳转后的地址</li>
</ol>
<h1><a name="t0"></a>1、换域名后导流到新域名</h1>
需要将之前用的www.a.com域名的流量全部跳转到www.b.com
实现效果：比如访问 www.a.com/123.html自动跳到www.b.com/123.html

我们用nginx实现

cd /etc/nginx/sites-available

vi mysite

增加rewrite命令
<pre>server {

    listen                   80;

    server_name      www.a.com;

    rewrite  ^/(.*)$     http://www.b.com/$1 permanent;

}</pre>
<h1><a name="t1"></a>2、主域名跳转到www域名</h1>
比如将主域名xxx.com 跳转到www.xxx.com
<pre>cd /etc/nginx/sites-available

vi mysite

#主域名跳转到www域名

server {

    listen                80;

    server_name    xxx.com;

    rewrite  ^/(.*)$  http://www.xxx.com/$1 permanent;

}

server {

    listen                80;

    server_name   www.xxx.com;

    #已省略余下通用配置内容

}</pre>
<h1><a name="t2"></a>3、主目录跳转，子目录不跳转</h1>
a.com和www.a.com都跳到www.b.com
<a href="http://www.a.com/123%E4%B8%8D%E8%B7%B3" target="_blank" rel="nofollow noopener">www.a.com/123不跳</a>
<pre>server {

        listen               80;

        server_name  www.a.us a.us;

        #根目录跳转

        location / {

                rewrite .+ http://www.b.com/ permanent;

        }

        #非根目录本地执行

        location ~* /.+ {

            #已省略余下通用配置内容

        }

}</pre>
4、301跳转设置：
<pre>server {
       listen                   80;
       server_name     123.com;
       rewrite ^/(.*) http://456.com/$1 permanent;
       access_log off;
}</pre>
permanent – 返回永久重定向的HTTP状态301

5、302跳转设置：
<pre>server {
       listen                    80;
       server_name      123.com;
       rewrite ^/(.*) http://456.com/$1 redirect;
       access_log off;
}</pre>
redirect – 返回临时重定向的HTTP状态302

6、禁止htaccess
<pre>location ~//.ht {

         deny all;

}</pre>
7、禁止多个目录
<pre>location ~ ^/(cron|templates)/ {

         deny all;

         break;

 }</pre>
8、禁止以/data开头的文件
可以禁止/data/下多级目录下.log.txt等请求;
<pre>location ~ ^/data {

         deny all;

}</pre>
9、禁止单个目录
不能禁止.log.txt能请求
<pre>location /searchword/cron/ {

         deny all;

 }</pre>
10、禁止单个文件
<pre>location ~ /data/sql/data.sql {

         deny all;

}</pre>
11、给favicon.ico和robots.txt设置过期时间;
这里为favicon.ico为99天,robots.txt为7天并不记录404错误日志
<pre>location ~(favicon.ico) {

                 log_not_found off;

                 expires 99d;

                 break;

}</pre>
&nbsp;
<pre>location ~(robots.txt) {

                 log_not_found off;

                 expires 7d;

                 break;

}</pre>
12、设定某个文件的过期时间;这里为600秒，并不记录访问日志
<pre>location ^~ /html/scripts/loadhead_1.js {

                 access_log   off;

                 root /opt/lampp/htdocs/web;

                 expires 600;

                 break;

}</pre>
13、文件反盗链并设置过期时间
这里的return 412 为自定义的http状态码，默认为403，方便找出正确的盗链的请求
“rewrite ^/ http://leech.c1gstudio.com/leech.gif;”显示一张防盗链图片
“access_log off;”不记录访问日志，减轻压力
“expires 3d”所有文件3天的浏览器缓存
<pre>location ~* ^.+/.(jpg|jpeg|gif|png|swf|rar|zip|css|js)$ {

valid_referers none blocked *.c1gstudio.com *.c1gstudio.net localhost 208.97.167.194;

if ($invalid_referer) {

    rewrite ^/ http://leech.c1gstudio.com/leech.gif;

    return 412;

    break;

}

    access_log   off;

    root /opt/lampp/htdocs/web;

    expires 3d;

    break;

}</pre>
14、只充许固定ip访问网站，并加上密码
<pre>root  /opt/htdocs/www;

allow   208.97.167.194;

allow   222.33.1.2;

allow   231.152.49.4;

deny    all;

auth_basic "C1G_ADMIN";

auth_basic_user_file htpasswd;</pre>
15、将多级目录下的文件转成一个文件，增强seo效果
<pre>/job-123-456-789.html 指向/job/123/456/789.html

rewrite ^/job-([0-9]+)-([0-9]+)-([0-9]+)/.html$ /job/$1/$2/jobshow_$3.html last;</pre>
16、将根目录下某个文件夹指向2级目录
如/shanghaijob/ 指向 /area/shanghai/
如果你将last改成permanent，那么浏览器地址栏显是/location/shanghai/

rewrite ^/([0-9a-z]+)job/(.*)$ /area/$1/$2 last;

上面例子有个问题是访问/shanghai 时将不会匹配

rewrite ^/([0-9a-z]+)job$ /area/$1/ last;

rewrite ^/([0-9a-z]+)job/(.*)$ /area/$1/$2 last;

这样/shanghai 也可以访问了，但页面中的相对链接无法使用，
如./list_1.html真实地址是/area/shanghia/list_1.html会变成/list_1.html,导至无法访问

那我加上自动跳转也是不行咯
(-d $request_filename)它有个条件是必需为真实目录，而我的rewrite不是的，所以没有效果

if (-d $request_filename){

rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;

}

知道原因后就好办了，让我手动跳转吧

rewrite ^/([0-9a-z]+)job$ /$1job/ permanent;

rewrite ^/([0-9a-z]+)job/(.*)$ /area/$1/$2 last;

17、文件和目录不存在的时候重定向：

if (!-e $request_filename) {

proxy_pass http://127.0.0.1;

}

18、域名跳转
<pre>server

     {

             listen       80;

             server_name  jump.c1gstudio.com;

             index index.html index.htm index.php;

             root  /opt/lampp/htdocs/www;

             rewrite ^/ http://www.c1gstudio.com/;

             access_log  off;

}</pre>
19、多域名转向
<pre>server_name  www.c1gstudio.com www.c1gstudio.net;

             index index.html index.htm index.php;

             root  /opt/lampp/htdocs;

             if ($host ~ "c1gstudio/.net") {

                 rewrite ^(.*) http://www.c1gstudio.com$1 permanent;

}</pre>
20、三级域名跳转
<pre>if ($http_host ~* "^(.*)/.i/.c1gstudio/.com$") {

    rewrite ^(.*) http://top.yingjiesheng.com$1;

    break;

}</pre>
</div>
</div>
</article>