---
ID: 1456
post_title: >
  基于Google Code Prettify
  wordpress插件WP-code-button
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/23/%e5%9f%ba%e4%ba%8egoogle-code-prettify-wordpress%e6%8f%92%e4%bb%b6wp-code-button/'
published: true
post_date: 2018-11-23 17:49:36
---
<header class="article-header">
<h1 class="article-title"><a href="https://www.phpsong.com/1645.html">基于Google Code Prettify wordpress插件WP-code-button</a></h1>
<div class="meta"><span class="muted"><a href="https://www.phpsong.com/wordpress"><i class="icon-list-alt icon12"></i> wordpress</a></span> <time class="muted"><i class="ico icon-time icon12"></i> 2015-11-12 01:16:55</time> <span class="muted"><i class="ico icon-eye-open icon12"></i> 2412浏览</span> <span class="muted"><i class="icon-comment icon12"></i> <a href="https://www.phpsong.com/1645.html#comments">0评论</a></span></div>
</header><article class="article-content">做自己写了一个代码插件<strong>WP-code-button</strong>，一个基于 Google Code Prettify 实现的WordPress代码高亮插件,后台带代码插入功能，无须设置安装就可以使用

Google Code Prettify 是居于jquery的代码高亮插件,所以你的博客必须安装加载jquery,插件中不包含jquery

主要是之前一直用Google Code Prettify来实现代码高亮，但是有问题就是wordpress默认tinymce编辑器后台代码插入的时候会把代码的缩减都去除，如果代码有多行那样看起来就不是很直观，现在开发WP-code-button wordpress插件来解决这个问题

目前版本0.1 <a title="" href="https://wordpress.org/plugins/wp-code-button/" target="_blank" rel="external nofollow noopener" data-original-title="">官方插件下载</a>
或者在wordpress 后台插件处搜索“WP-code-button”

github链接: https://github.com/QiuCarson/wp-code-button

<strong>更新日志</strong>

V 0.4
添加代码换行样式，如果没有jquery就添加jqeruy

V 0.3

1、修改样式

V 0.2

1、修改js底部加载
V 0.1

1、解决基于 Google Code Prettify 实现的WordPress代码高亮插件,后台带代码插入功能
<h2>①后台</h2>
插件安装好之后后台wordpress默认tinymce编辑器会出现代码插入按钮

<a class="highslide-image" title="" href="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201084848571.png" target="_blank" rel="nofollow noopener" data-original-title=""><img class="alignnone wp-image-1646 size-full" src="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201084848571.png" sizes="(max-width: 692px) 100vw, 692px" srcset="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201084848571.png 692w, https://static.phpsong.com/wp-content/uploads/2015/11/2015111201084848571-300x64.png 300w" alt="tinymce编辑器代码插入按钮" width="692" height="147" /></a>

单击之后弹出<strong>WP-code-button</strong>代码插入界面

<a class="highslide-image" title="" href="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201130393622.png" target="_blank" rel="nofollow noopener" data-original-title=""><img class="alignnone wp-image-1649 size-full" src="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201130393622.png" sizes="(max-width: 573px) 100vw, 573px" srcset="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201130393622.png 573w, https://static.phpsong.com/wp-content/uploads/2015/11/2015111201130393622-300x287.png 300w" alt="WP-code-button代码插入界面" width="573" height="549" /></a>
<h2>②前台</h2>
<a class="highslide-image" title="" href="https://static.phpsong.com/wp-content/uploads/2015/11/2015120103590010183.png" target="_blank" rel="nofollow noopener" data-original-title=""><img class="alignnone size-full wp-image-1771" src="https://static.phpsong.com/wp-content/uploads/2015/11/2015120103590010183.png" sizes="(max-width: 764px) 100vw, 764px" srcset="https://static.phpsong.com/wp-content/uploads/2015/11/2015120103590010183.png 764w, https://static.phpsong.com/wp-content/uploads/2015/11/2015120103590010183-300x113.png 300w" alt="代码高亮插件WP-code-button前天样式" width="764" height="287" /></a>

插件中Google Code Prettify的css插入到wp_head里，js插入到wp_footer，以我的小松博客为例，代码截图如下

<a class="highslide-image" title="" href="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201090052835.png" target="_blank" rel="nofollow noopener" data-original-title=""><img class="alignnone wp-image-1647 size-full" src="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201090052835.png" sizes="(max-width: 733px) 100vw, 733px" srcset="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201090052835.png 733w, https://static.phpsong.com/wp-content/uploads/2015/11/2015111201090052835-300x102.png 300w" alt="WP-code-button css前台代码插入" width="733" height="250" /></a> <a class="highslide-image" title="" href="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201090053524.png" target="_blank" rel="nofollow noopener" data-original-title=""><img class="alignnone wp-image-1648 size-full" src="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201090053524.png" sizes="(max-width: 675px) 100vw, 675px" srcset="https://static.phpsong.com/wp-content/uploads/2015/11/2015111201090053524.png 675w, https://static.phpsong.com/wp-content/uploads/2015/11/2015111201090053524-300x84.png 300w" alt="WP-code-button js前台代码插入" width="675" height="190" /></a>

<span id="baiduguanggao">QQ交流群：136351212(满) 455721967</span>如无特别说明，本站文章皆为原创，若要转载，务必请注明以下原文信息:
转载保留版权：<a title="" href="https://www.phpsong.com/" data-original-title="">小松博客</a>» <a title="" href="https://www.phpsong.com/1645.html" data-original-title="">基于Google Code Prettify wordpress插件WP-code-button</a>
本文链接地址：<a title="" href="https://www.phpsong.com/1645.html" rel="bookmark" data-original-title="基于Google Code Prettify wordpress插件WP-code-button">https://www.phpsong.com/1645.html</a>

</article>