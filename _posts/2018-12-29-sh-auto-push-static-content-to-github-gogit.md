---
ID: 1817
post_title: >
  sh auto push static content to github
  -gogit
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/12/29/sh-auto-push-static-content-to-github-gogit/
published: true
post_date: 2018-12-29 18:18:55
---
<div class="post-topheader custom- pt0">
<div class="mb20">
<div class="block-for-right-border">
<div class="row">
<div class="col-md-12 col-sm-12 col-xs-12">
<div class="post-topheader__info" data-username="HavenShen" data-userslug="havenshen" data-useravatar="https://avatar-static.segmentfault.com/144/846/1448463963-5926ba6a8fd39_big64">
<div class="article__author clearfix">
<div class="article__authorright">2016-03-28 发布</div>
</div>
<h1 class="h1 post-topheader__info--title" data-id="1190000004701784"><a href="https://segmentfault.com/a/1190000004701784">一个结合crontab定时推送github或coding库小玩意 </a></h1>
<h1 id="articleHeader0">gogit</h1>
<h1 id="articleTitle" class="h1 post-topheader__info--title" data-id="1190000004701784"></h1>
<div class="content__tech hidden-xs"> 2.5k 次阅读  ·  读完需要 4 分钟</div>
</div>
</div>
</div>
</div>
</div>
</div>
<div class="visible-lg">
<div class="side-widget">
<div id="side-widget-votes-btn" class="stream__item-zan btn btn-default mt0 mb15 ml0 mr0 pt0 pb0 pl0 pr0 "><span id="side-widget-votes-num" class="stream__item-zan-number">0</span></div>
<i id="side-widget-bookmarks-btn" class="fa fa-bookmark item "></i><i class="fa fa-weibo item"></i><i class="fa fa-weixin item" title="" data-toggle="popover" data-placement="right" data-original-title=""></i><i class="fa fa-twitter item"></i><i class="fa fa-facebook item"></i><i class="fa fa-arrow-up item"></i></div>
</div>
<div class="article fmt article__content" data-id="1190000004701784" data-license="cc">
<h1 id="articleHeader0">gogit</h1>
<blockquote>一个结合crontab定时推送github或coding库小玩意。</blockquote>
Github:<a href="https://github.com/HavenShen/gogit" target="_blank" rel="nofollow noopener noreferrer">https://github.com/HavenShen/gogit</a>

注：运行此玩意的电脑，必须可运行python、已经配置好github和coding使用ssh key 无密钥通道git的ssh获取方式(推荐使用常年不关机的linux服务器)。

配置参考：<a href="http://www.cnblogs.com/havenshen/p/3493522.html" target="_blank" rel="nofollow noopener noreferrer">Git配置安装使用教程操作github上传克隆数据</a>
<h2 id="articleHeader1">安装</h2>
1.克隆此库
<pre class="shell hljs"><code class="shell">git clone git@github.com:HavenShen/gogit.git</code></pre>
<h2 id="articleHeader2">配置推送github同时提交coding库</h2>
1.在自己的github和coding中创建自己的新库

可取名如：<code>mygogit</code>取得自己的ssh地址
<ul>
 	<li><code>git@github.com:xxx/mygogit.git</code></li>
 	<li><code>git@git.coding.net:xxx/mygogit.git</code></li>
</ul>
2.修改及增加刚在github克隆的库目录下<code>gogit/.git/config</code>文件中的<code>[remote "origin]"</code>节点下<code>url</code>路径
<pre class="shell hljs"><code class="shell">url = git@github.com:xxx/mygogit.git
url = git@git.coding.net:xxx/mygogit.git</code></pre>
<h2 id="articleHeader3">设置<code>crontab</code>定时任务</h2>
<pre class="shell hljs"><code class="shell"><span class="hljs-meta">
#</span><span class="bash">编辑定时任务</span>

crontab -e
<span class="hljs-meta">
#</span><span class="bash">键入每天下午3点执行命令</span>

00 15 * * * python /home/gitfile/gogit/main.py #这边执行路径按自己的库目录而改动
<span class="hljs-meta">
#</span><span class="bash">保存退出</span>

:wq
  </code></pre>
搞定。

坐等任务每天帮你填补github空地，以及coding每天推送代码的 + 0.01码币
<h2 id="articleHeader4">错误反馈</h2>
1.如果crontab不执行python脚本

在<code>main.py</code>文件头部加入
<pre class="shell hljs"><code class="shell"><span class="hljs-meta">#</span><span class="bash">!/usr/bin/python <span class="hljs-comment">#对应python环境变量路径</span></span></code></pre>
把Python（<code>main.py</code>）的属性改为可执行
<pre class="shell hljs"><code class="shell">chmod a+x main.py</code></pre>
修改<code>crontab</code>
<pre class="shell hljs"><code class="shell">crontab -e
00 15 * * * /home/gitfile/gogit/main.py</code></pre>
<h2 id="articleHeader5">License</h2>
</div>