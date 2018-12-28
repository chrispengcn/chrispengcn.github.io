---
ID: 1367
post_title: >
  CentOs7 安装Tengine
  并设置成系统服务，开机自动启动。
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/09/14/centos7-%e5%ae%89%e8%a3%85tengine-%e5%b9%b6%e8%ae%be%e7%bd%ae%e6%88%90%e7%b3%bb%e7%bb%9f%e6%9c%8d%e5%8a%a1%ef%bc%8c%e5%bc%80%e6%9c%ba%e8%87%aa%e5%8a%a8%e5%90%af%e5%8a%a8%e3%80%82/'
published: true
post_date: 2018-09-14 09:17:11
---
<div class="article-header-box">
<div class="article-header">
<div class="article-title-box">
<h1 class="title-article">CentOs7 安装Tengine 并设置成系统服务，开机自动启动。</h1>
</div>
<div class="article-info-box">
<div class="article-bar-top">

<span class="time">2016年07月12日 15:44:29</span> <a class="follow-nickName" href="https://me.csdn.net/nimasike" target="_blank" rel="noopener">温故而知新666</a> <span class="read-count">阅读数：5422</span><span class="article_info_click">更多</span>
<div class="tags-box space"><span class="label">个人分类：</span> <a class="tag-link" href="https://blog.csdn.net/nimasike/article/category/6250854" target="_blank" rel="noopener">Linux</a></div>
</div>
<div class="operating"></div>
</div>
</div>
</div>
<article>
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
<div class="article-copyright">版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/nimasike/article/details/51889171</div>
<div class="htmledit_views">

本文使用<a href="http://tengine.taobao.org/download/tengine-2.1.2.tar.gz" target="_blank" rel="nofollow noopener">Tengine-2.1.2.tar.gz</a>  官方下载地址：http://tengine.taobao.org/download_cn.html。

http://tengine.taobao.org/nginx_docs/cn/docs/    文档
<h1>1、安装Tenqine</h1>
<h2>1.1安装 pcre-8.38</h2>
1：下载地址: https://sourceforge.net/projects/pcre/files/pcre/

2：解压缩<span lang="en-us" xml:lang="en-us">pcre-xx.tar.gz</span>包， tar -zxvf  pcre-xx.tar.gz

3：进入解压缩目录，执行<span lang="en-us" xml:lang="en-us">./configure</span>。

4：make &amp; make install
<h2>1.2安装zlib 1.2.8</h2>
1：获取编译安装包http://www.zlib.net/

2：解压缩zlib-1.2.8.tar.gz    tar -zxvf zlib-1.2.8.tar.gz

3：进入解压缩目录，执行<span lang="en-us" xml:lang="en-us">./configure</span>。

4：make &amp; make install
<h2>1.3安装openssl</h2>
<div>推荐使用 YUM安装 y um install openssl  和 yum install openssl-devel。</div>
<div>其它安装方式：</div>
1.获取openssl编译安装包，在http://www.openssl.org/source/上可以获取当前最新的版本。

2.解压缩openssl-xx.tar.gz包。

3.进入解压缩目录，执行./config

4.make &amp; make install
<h2>1.4安装tengine</h2>
<div>1：下载稳定版本的软件包 官方网址:http://tengine.taobao.org/download_cn.html</div>
<div>2：解压缩软件包tengine-2.1.2.tar.gz   tar -zxvf tengine-2.1.2.tar.gz</div>
<div>3：进入解压缩目录 cd zxvf tengine-2.1.2</div>
<div>4：然后执行 ./configure --prefix=/usr/local/nginx   （说明:--prefix=/usr/local/nginx是安装路径，不默认就是这个，可以自定义位置）</div>
</div>
<div>
<pre>企业标准安装

./configure \
--prefix=/usr\
--sbin-path=/usr/sbin/nginx\
--conf-path=/etc/nginx/nginx.conf\
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--pid-path=/var/run/nginx/nginx.pid\
--lock-path=/var/lock/nginx.lock\
--user=nginx\
--group=nginx\
--with-http_ssl_module\
--with-http_flv_module\
--with-http_stub_status_module\
--with-http_gzip_static_module\
--http-client-body-temp-path=/var/tmp/nginx/client/ \
--http-proxy-temp-path=/var/tmp/nginx/proxy/ \
--http-fastcgi-temp-path=/var/tmp/nginx/fcgi/ \
--http-uwsgi-temp-path=/var/tmp/nginx/uwsgi\
--http-scgi-temp-path=/var/tmp/nginx/scgi\
--with-pcre</pre>
</div>
<div class="htmledit_views">

5：make &amp; make install  完成安装

6：进入cd /usr/local/nginx/   然后查看安装后的目录 ls -l

<img src="https://img-blog.csdn.net/20160712152512519?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" />

7：手动启动Nginx    /usr/local/nginx/sbin/nginx

ln -s /usr/local/nginx/sbin/nginx /etc/init.d/

&nbsp;
<h1>2、设置为系统服务</h1>
<div>1：系统用户登录系统后启动的服务 的目录</div>
<div>/usr/lib/systemd/system</div>
2：如需要开机没有登陆情况下就能运行的程序在系统目录内

/lib/systemd/system

3：我希望系统开机就启动目录，所以我把文件放在系统目录内。

vim /lib/systemd/system/nginx.service 创建文件
<ul class="hljs-ln">
 	<li>
<div class="hljs-ln-numbers"> [Unit]</div></li>
 	<li>
<div class="hljs-ln-numbers"> Description=The nginx HTTP and reverse proxy server</div></li>
 	<li>
<div class="hljs-ln-numbers"> After=syslog.target network.target remote-fs.target nss-lookup.target</div></li>
 	<li>
<div class="hljs-ln-numbers"></div></li>
 	<li>
<div class="hljs-ln-numbers"> [Service]</div></li>
 	<li>
<div class="hljs-ln-numbers"> Type=forking</div></li>
 	<li>
<div class="hljs-ln-numbers"> PIDFile=/usr/local/nginx/logs/nginx.pid</div></li>
 	<li>
<div class="hljs-ln-numbers"> ExecStartPre=/usr/local/nginx/sbin/nginx -t</div></li>
 	<li>
<div class="hljs-ln-numbers"> ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf</div></li>
 	<li>
<div class="hljs-ln-numbers"> ExecReload=/bin/kill -s HUP $MAINPID</div></li>
 	<li>
<div class="hljs-ln-numbers"> ExecStop=/bin/kill -s QUIT $MAINPID</div></li>
 	<li>
<div class="hljs-ln-numbers"> PrivateTmp=<span class="hljs-keyword">true</span></div></li>
 	<li>
<div class="hljs-ln-numbers"></div>
<div class="hljs-ln-code"></div></li>
 	<li>
<div class="hljs-ln-numbers"> [Install]</div></li>
 	<li>
<div class="hljs-ln-numbers"> WantedBy=multi-user.target</div></li>
</ul>
4、修改文件权限
<pre><code class="language-java hljs">chmod <span class="hljs-number">745</span> nginx.service </code></pre>
5、设置为开机启动
<pre><code class="language-java hljs">systemctl enable nginx.service</code></pre>
<h1>3、其它命令</h1>
启动nginx服务
<pre>systemctl start nginx.service</pre>
设置开机自启动
<pre>systemctl enable nginx.service</pre>
停止开机自启动
<pre>systemctl disable nginx.service</pre>
查看服务当前状态
<pre>systemctl status nginx.service</pre>
重新启动服务
<pre>systemctl restart nginx.service</pre>
查看所有已启动的服务
<pre>systemctl list-units --type=service</pre>
参考资料:

http://www.cnblogs.com/skynet/p/4146083.html

http://www.centoscn.com/CentOS/config/2015/0507/5374.html

http://down.chinaz.com/server/201112/1476_1.htm

http://www.cnblogs.com/jianxie/p/3990377.html

</div>
</div>
</article>