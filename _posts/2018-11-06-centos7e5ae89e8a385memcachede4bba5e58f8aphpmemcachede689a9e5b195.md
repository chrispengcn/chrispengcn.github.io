---
ID: 1411
post_title: >
  centos7安装memcached以及phpmemcached扩展
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/06/centos7%e5%ae%89%e8%a3%85memcached%e4%bb%a5%e5%8f%8aphpmemcached%e6%89%a9%e5%b1%95/'
published: true
post_date: 2018-11-06 18:28:34
---
<div class="article-header-box">
<div class="article-header">
<div class="article-title-box">
<h1 class="title-article">centos7安装memcached以及phpmemcached扩展</h1>
</div>
<div class="article-info-box">
<div class="article-bar-top"><span class="time">2018年01月09日 13:59:41</span> 卫忠梁 <span class="read-count">阅读数：555</span></div>
<div class="operating"></div>
</div>
</div>
</div>
<article>
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
<div class="htmledit_views">
<div><strong>1.安装memcached:</strong></div>
<div>yum -y install memcached</div>
<div><strong>2.设置memcached开机启动:</strong></div>
<div>chkconfig memcached on</div>
<div><strong>3.立即启动memcached服务:</strong></div>
<div>service memcached start</div>
<div><strong>4.查找memcached安装位置:</strong></div>
<div>rpm -ql memcached</div>
<div><strong>5.查看memcached配置文件:</strong></div>
<div>cat /etc/sysconfig/memcached</div>
<div><strong>6.</strong><strong>执行</strong></div>
<div>netstat -tunlp | grep memcached</div>
<div>看到11211端口，说明memcached安装成功。</div>
<div><strong>7.安装libmemached:</strong></div>
<div>wget https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz</div>
<div>tar -zxvf libmemcached-1.0.18.tar.gz</div>
<div>cd libmemcached-1.0.18/</div>
<div>./configure --prefix=/usr/lib/libmemcached</div>
<div>make &amp;&amp; make install</div>
<div><strong>8.</strong><strong>下载memcache扩展包并安装:(</strong><strong>安装git</strong><strong>: yum -y install git</strong><strong>)</strong></div>
<div>git clone <a href="https://github.com/php-memcached-dev/php-memcached.git" target="_blank" rel="nofollow noopener">https://github.com/php-memcached-dev/php-memcached.git</a></div>
<div></div>
<div>cd php-memcached/</div>
<div></div>
<div>git checkout php7</div>
<div></div>
<div>/usr/local/php716/bin/phpize</div>
<div></div>
<div>./configure -enable-memcached -with-php-config=/usr/local/php716/bin/php-config -with-zlib-dir -with-libmemcached-dir=/usr/lib/libmemcached -prefix=/usr/local/phpmemcached --disable-memcached-sasl</div>
<div></div>
<div>make &amp;&amp; make install</div>
<div></div>
<div><strong>9.修改php.ini</strong>vim /usr/local/php716/lib/php.ini</div>
<div>#在END前加上extension=memcached.so;# 重启php-fpm</div>
<div></div>
<div><strong>注意:如果发现找不到php.ini文件,解决方法:</strong></div>
<div><strong>&lt;&lt;&lt;--------------&gt;&gt;&gt;</strong></div>
<div>去原php的源码目录(就是刚开始下载的php压缩包)查看<strong>php.ini-development或php.ini-production</strong></div>
<div>再当前环境选择将哪个文件<strong>重命名为php.ini</strong></div>
<div>并同时将php.ini文件<strong>复制</strong>到phpinfo()中php.ini所在的目录中即可</div>
<div>(在phpinfo里面搜索php.ini,查看php.ini应该在哪个路径)</div>
<div><strong>&lt;&lt;&lt;--------------&gt;&gt;&gt;</strong></div>
</div>
</div>
</article>