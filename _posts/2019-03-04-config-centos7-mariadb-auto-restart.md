---
ID: 2464
post_title: >
  如何配置CentOS7
  mariadb服务在崩溃或重启后自动启动
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/03/04/config-centos7-mariadb-auto-restart/
published: true
post_date: 2019-03-04 13:30:39
---
<div class="article-header-box">
<div class="article-header">
<div class="article-title-box">
<h1 class="title-article">对于centos7 mariadb方式归纳：</h1>
</div>
</div>
</div>
<article class="baidu_pl">
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
<div id="content_views" class="htmledit_views">
<div><strong>重启reboot后自动开启服务</strong>：</div>
<div>查看是否设置systemd is-enabled mariadb</div>
<div>如果未设置执行：</div>
<div>systemd enable mariadb</div>
<div><strong>crash崩溃后自动启动服务</strong>：</div>
<pre>编辑vi /etc/systemd/system/multi-user.target.wants/mariadb.service</pre>
<pre>在[Service] 节中增加Restart=always</pre>
<div>退出编辑，执行systemctl daemon-reload</div>
<div>测试是否成功设置，</div>
<div>sudo systemctl status mariadb.service</div>
<div>找到pid</div>
<div>Main PID: 661 (mysqld_safe)</div>
<div>杀掉进程 kill -9 661，再次查看服务状态</div>
<div>sudo systemctl status mysqld.service</div>
<div>pid发生变化</div>
<div>Main PID: 11217 (mysqld_safe)</div>
<div>证明启动成功。</div>
</div>
</div>
</article>