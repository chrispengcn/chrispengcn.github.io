---
ID: 1425
post_title: 'magento二次开发 &#8211; 在magento中配置使用redis缓存'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/07/magento%e4%ba%8c%e6%ac%a1%e5%bc%80%e5%8f%91-%e5%9c%a8magento%e4%b8%ad%e9%85%8d%e7%bd%ae%e4%bd%bf%e7%94%a8redis%e7%bc%93%e5%ad%98/'
published: true
post_date: 2018-11-07 16:05:21
---
<div class="article">
<h1 class="title">magento二次开发 - 在magento中配置使用redis缓存</h1>
<div class="show-content" data-note-content="">
<div class="show-content-free">

php常用的缓存工具：memcached和redis,本文讲的是在magento框架中借助magento的模块来使用redis
<blockquote>
<ul>
 	<li>准备工作</li>
 	<li>magento中配置redis</li>
 	<li>可能遇到的问题</li>
</ul>
</blockquote>
<h2>准备工作</h2>
确保你的电脑安装并启动了redis服务、配置了php的redis扩展
若没有，查看此文<a href="https://www.jianshu.com/p/af33284aa57a" target="_blank" rel="noopener">《mac下安装配置redis》</a>
<h2>magento配置reids</h2>
<ol>
 	<li>确保magento框架中安装并启用了<code>Cm_Cache_Backend_Redis</code>模块</li>
 	<li>修改以下路径中的local配置文件
<code>项目根目录/app/etc/local.xml</code>
在<code>config/global</code>下添加子节点<code>cache</code>如下</li>
</ol>
<pre class="hljs xml"><code class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">cache</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">backend</span>&gt;</span>Cm_Cache_Backend_Redis<span class="hljs-tag">&lt;/<span class="hljs-name">backend</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">backend_options</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">server</span>&gt;</span>127.0.0.1<span class="hljs-tag">&lt;/<span class="hljs-name">server</span>&gt;</span> <span class="hljs-comment">&lt;!-- or absolute path to unix socket --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">port</span>&gt;</span>6379<span class="hljs-tag">&lt;/<span class="hljs-name">port</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">persistent</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">persistent</span>&gt;</span> <span class="hljs-comment">&lt;!-- Specify unique string to enable persistent connections. E.g.: sess-db0; bugs with phpredis and php-fpm are known: https://github.com/nicolasff/phpredis/issues/70 --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">database</span>&gt;</span>0<span class="hljs-tag">&lt;/<span class="hljs-name">database</span>&gt;</span> <span class="hljs-comment">&lt;!-- Redis database number; protection against accidental data loss is improved by not sharing databases --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">password</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">password</span>&gt;</span> <span class="hljs-comment">&lt;!-- Specify if your Redis server requires authentication --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">force_standalone</span>&gt;</span>0<span class="hljs-tag">&lt;/<span class="hljs-name">force_standalone</span>&gt;</span>  <span class="hljs-comment">&lt;!-- 0 for phpredis, 1 for standalone PHP --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">connect_retries</span>&gt;</span>1<span class="hljs-tag">&lt;/<span class="hljs-name">connect_retries</span>&gt;</span>    <span class="hljs-comment">&lt;!-- Reduces errors due to random connection failures; a value of 1 will not retry after the first failure --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">read_timeout</span>&gt;</span>10<span class="hljs-tag">&lt;/<span class="hljs-name">read_timeout</span>&gt;</span>         <span class="hljs-comment">&lt;!-- Set read timeout duration; phpredis does not currently support setting read timeouts --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">automatic_cleaning_factor</span>&gt;</span>0<span class="hljs-tag">&lt;/<span class="hljs-name">automatic_cleaning_factor</span>&gt;</span> <span class="hljs-comment">&lt;!-- Disabled by default --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">compress_data</span>&gt;</span>1<span class="hljs-tag">&lt;/<span class="hljs-name">compress_data</span>&gt;</span>  <span class="hljs-comment">&lt;!-- 0-9 for compression level, recommended: 0 or 1 --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">compress_tags</span>&gt;</span>1<span class="hljs-tag">&lt;/<span class="hljs-name">compress_tags</span>&gt;</span>  <span class="hljs-comment">&lt;!-- 0-9 for compression level, recommended: 0 or 1 --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">compress_threshold</span>&gt;</span>20480<span class="hljs-tag">&lt;/<span class="hljs-name">compress_threshold</span>&gt;</span>  <span class="hljs-comment">&lt;!-- Strings below this size will not be compressed --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">compression_lib</span>&gt;</span>gzip<span class="hljs-tag">&lt;/<span class="hljs-name">compression_lib</span>&gt;</span> <span class="hljs-comment">&lt;!-- Supports gzip, lzf, lz4 (as l4z) and snappy --&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">use_lua</span>&gt;</span>0<span class="hljs-tag">&lt;/<span class="hljs-name">use_lua</span>&gt;</span> <span class="hljs-comment">&lt;!-- Set to 1 if Lua scripts should be used for some operations --&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">backend_options</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">cache</span>&gt;</span>
</code></pre>
<ol start="3">
 	<li>清空magento缓存使得配置文件生效，关于如何清空magento缓存，参考<a href="https://www.jianshu.com/p/a240602a4f36" target="_blank" rel="noopener">《magento二次开发 - 如何清除magento缓存》</a></li>
</ol>
<h2>可能遇到的问题</h2>
6.22:
今天上午打开本地网站时报如下错误<code>connection to Redis failed</code>：
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="491" data-height="185"><img class="" src="https://upload-images.jianshu.io/upload_images/1903856-73c60ab2ef05b45b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/491/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/1903856-73c60ab2ef05b45b.png" data-original-width="491" data-original-height="185" data-original-format="image/png" data-original-filesize="49162" /></div>
</div>
<div class="image-caption">错误页面</div>
</div>
发现是因为redis服务没有开启导致的，开启redis即可

<strong><em>说明magento在配置了redis之后那么要保持reids服务一直处于开启状态</em></strong>

参考：
[1]在magento中配置redis文档：
<blockquote><a href="https://link.jianshu.com/?t=http://devdocs.magento.com/guides/m1x/ce18-ee113/using_redis.html" target="_blank" rel="nofollow noopener">http://devdocs.magento.com/guides/m1x/ce18-ee113/using_redis.html</a></blockquote>
[2]Cm_Cache_Backend_Redis配置
<blockquote><a href="https://link.jianshu.com/?t=https://github.com/colinmollenhour/Cm_Cache_Backend_Redis/blob/master/README.md" target="_blank" rel="nofollow noopener">https://github.com/colinmollenhour/Cm_Cache_Backend_Redis/blob/master/README.md</a></blockquote>
</div>
</div>
</div>
<div id="free-reward-panel" class="support-author"></div>