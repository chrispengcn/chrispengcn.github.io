---
ID: 1325
post_title: >
  Nginx如何设置拒绝或允许指定ip访问
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/08/29/nginx%e5%a6%82%e4%bd%95%e8%ae%be%e7%bd%ae%e6%8b%92%e7%bb%9d%e6%88%96%e5%85%81%e8%ae%b8%e6%8c%87%e5%ae%9aip%e8%ae%bf%e9%97%ae/'
published: true
post_date: 2018-08-29 17:50:09
---
<div class="article-header-box">
<div class="article-header">
<div class="article-title-box">
<h1 class="title-article">Nginx如何设置拒绝或允许指定ip访问</h1>
</div>
<div class="article-info-box">
<div class="operating"> <span style="line-height: inherit;"> </span></div>
</div>
</div>
</div>
<article>
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
<div class="htmledit_views">nginx拒绝或允许指定IP,是使用模块HTTP访问控制模块（HTTP Access）.
控制规则按照声明的顺序进行检查，首条匹配IP的访问规则将被启用。</div>
<div></div>
<div class="htmledit_views">location / {
deny    192.168.1.1;
allow   192.168.1.0/24;
allow   10.1.1.0/16;
deny    all;
}
上面的例子中仅允许192.168.1.0/24和10.1.1.0/16网络段访问这个location字段，但192.168.1.1是个例外。
注意规则的匹配顺序，如果你使用过apache你可能会认为你可以随意控制规则的顺序并且他们能够正常的工作，但实际上不行。

下面的这个例子将拒绝掉所有的连接：

location / {
#这里将永远输出403错误。
deny all;
#这些指令不会被启用，因为到达的连接在第一条已经被拒绝
deny    192.168.1.1;
allow   192.168.1.0/24;
allow   10.1.1.0/1
}

转至：<a href="http://www.server110.com/nginx/201311/3404.html" target="_blank" rel="nofollow noopener">http://www.server110.com/nginx/201311/3404.html</a>

</div>
</div>
</article>