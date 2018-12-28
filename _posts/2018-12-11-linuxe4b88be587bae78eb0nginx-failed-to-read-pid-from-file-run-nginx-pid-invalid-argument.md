---
ID: 1701
post_title: 'linux下出现nginx Failed to read PID from file /run/nginx.pid: Invalid argument'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/11/linux%e4%b8%8b%e5%87%ba%e7%8e%b0nginx-failed-to-read-pid-from-file-run-nginx-pid-invalid-argument/'
published: true
post_date: 2018-12-11 13:09:16
---
<div class="article-header-box">
<div class="article-header">
<div class="article-title-box">
<h1 class="title-article">linux下出现nginx Failed to read PID from file /run/nginx.pid: Invalid argument</h1>
</div>
<div class="article-info-box">
<div class="operating"></div>
</div>
</div>
</div>
<article class="baidu_pl">
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
<div id="content_views" class="htmledit_views">

解决方法：

mkdir -p /etc/systemd/system/nginx.service.d
printf "[Service]\nExecStartPost=/bin/sleep 0.1\n" &gt; /etc/systemd/system/nginx.service.d/override.conf

然后
systemctl daemon-reload
systemctl restart nginx.service

</div>
</div>
</article>