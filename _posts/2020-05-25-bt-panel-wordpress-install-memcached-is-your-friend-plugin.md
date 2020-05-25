---
ID: 3592
post_title: >
  bt panel wordpress install Memcached is
  your friend plugin
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2020/05/25/bt-panel-wordpress-install-memcached-is-your-friend-plugin/
published: true
post_date: 2020-05-25 15:50:01
---
在没有使用内存缓存之前，白天博客不知道页面加载速度有多慢。而就在前段时间，给白天博客(wordoress站点)这个网站运行宝塔面板里的PHP扩展Memcached后，页面加载速度不知道提升了多少倍(初步估算3-5倍?)。那么本篇文章白天就将配置方法分享给大家。
<figure class="wp-block-image size-large"><img class="wp-image-1396 j-lazy" src="https://img.seobti.com/wp-content/uploads/2020/01/memcached.jpeg" alt="宝塔下借助 Memcached 加速wordpressw网站(附插件配置方法)" data-original="https://img.seobti.com/wp-content/uploads/2020/01/memcached.jpeg" /></figure>
<h2>宝塔面板安装 Memcached 扩展</h2>
在左侧菜单栏进入软件商店，搜索栏里搜索“<code>memcached</code>”并安装。
<figure class="wp-block-image size-large"><img class="wp-image-1392 j-lazy" src="https://img.seobti.com/wp-content/uploads/2020/01/memcached_1.png" alt="宝塔下借助 Memcached 加速wordpressw网站(附插件配置方法)" data-original="https://img.seobti.com/wp-content/uploads/2020/01/memcached_1.png" /></figure>
再到自己wordpress站点使用的PHP版本里点击设置后在安装扩展里安装 “<code>memcached</code>” 。
<figure class="wp-block-image size-large"><img class="wp-image-1393 j-lazy" src="https://img.seobti.com/wp-content/uploads/2020/01/memcached_3.png" alt="宝塔下借助 Memcached 加速wordpressw网站(附插件配置方法)" data-original="https://img.seobti.com/wp-content/uploads/2020/01/memcached_3.png" /></figure>
注：
<ol>
 	<li>安装成功后，以防万一可以通过phpinfo来检测已安装的扩展中是否已有 memcached 。</li>
 	<li>PHP 扩展中有 Memcache 扩展 和 Memcached 两个扩展，两者仅仅相差一个字母 D，别安装错了。</li>
</ol>
<h2>WordPress安装启用Memcached</h2>
1、下载 WordPress 中的 <code>Memcached Is Your Friend</code> 插件到本地。

PHP Memcached 插件官网下载地址：https://wordpress.org/plugins/memcached-is-your-friend/。

考虑到国内访问<a href="https://www.seobti.com/1330.html">WordPress官网429错误</a>，所以白天已将插件下载到网盘供大家下载：<a href="https://545c.com/file/21890530-425173344" target="_blank" rel="noreferrer noopener nofollow" aria-label="memcached-is-your-friend.zip（在新窗口打开）">memcached-is-your-friend.zip</a>

2、插件下载后解压到本地，把文件里的 <code>memcached-class-object-cache.php</code> 重命名为 <code>object-cache.php</code> 后再上传至 /wp-content/ 目录。(注：不是上传到 wp-content/plugins/ 目录)。

WordPress 会自动检查在 wp-content 目录下是否有 object-cache.php 文件，如果有，直接调用它作为 WordPress 对象缓存机制。

上述所说步骤做完之后，编辑博客根目录的wp-config.php 文件，添加下方两段代码进去并保存：
<pre class="wp-block-preformatted"><span class="c1">//是激活Batcache（功能说明https://www.seobti.com/1388.html）</span>
<span class="nx">define</span><span class="p">(</span><span class="s1">'ENABLE_CACHE'</span><span class="p">,</span> <span class="kc">true</span><span class="p">);</span>
<span class="c1">//这段是激活Memcached（功能说明https://www.seobti.com/1388.html）</span>
<span class="nx">define</span><span class="p">(</span><span class="s1">'WP_CACHE'</span><span class="p">,</span> <span class="kc">true</span><span class="p">);</span></pre>
白天截图给大家看下：
<figure class="wp-block-image size-large"><img class="wp-image-1391 j-lazy" src="https://img.seobti.com/wp-content/uploads/2020/01/2-1.png" alt="宝塔下借助 Memcached 加速wordpressw网站(附插件配置方法)" data-original="https://img.seobti.com/wp-content/uploads/2020/01/2-1.png" /></figure>
上方步骤完成之后，再到博客后台 – 插件 – 已安装插件，如下图所示，如果已安装插件中有显示”Memcached Is Your Friend”就表示已经开启了 Memcached 缓存功能
<figure class="wp-block-image size-large"><img class="wp-image-1403 j-lazy" src="https://img.seobti.com/wp-content/uploads/2020/01/2020010509541176.png" alt="宝塔下借助 Memcached 加速wordpressw网站(附插件配置方法)" data-original="https://img.seobti.com/wp-content/uploads/2020/01/2020010509541176.png" /></figure>
到前端试试页面打开速度，是不是飞快?