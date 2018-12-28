---
ID: 1165
post_title: 'Linux解决Warning: mysql_connect(): Headers and client library minor version mismatch. 警告'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/08/linux%e8%a7%a3%e5%86%b3warning-mysql_connect-headers-and-client-library-minor-version-mismatch-%e8%ad%a6%e5%91%8a/'
published: true
post_date: 2018-05-08 20:20:19
---
<div class="article-title-box">
<h6 class="title-article">Linux解决Warning: mysql_connect(): Headers and client library minor version mismatch. 警告</h6>
</div>
<div class="article-info-box">
<div class="article-bar-top d-flex">

<span class="time">2016年10月11日 17:47:44</span>
<div class="float-right"><span class="read-count">阅读数：6678</span></div>
</div>
</div>
<article>
<div id="article_content" class="article_content csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
<div class="htmledit_views">

<strong>丨</strong>版权说明<strong> </strong>: 《<a href="http://www.icheny.cn/linux%E8%A7%A3%E5%86%B3warning-mysql_connect-headers-and-client-library-minor-version-mismatch-%E8%AD%A6%E5%91%8A/" target="_blank" rel="noopener noreferrer">Linux解决Warning: mysql_connect(): Headers and client library minor version mismatch. 警告</a>》<strong>于当前</strong><a href="http://blog.csdn.net/ausboyue/" target="_blank" rel="noopener noreferrer">CSDN博客</a><strong>和</strong><a href="http://www.icheny.cn/" target="_blank" rel="noopener noreferrer">乘月网</a><strong>属同一原创，转载请说明出处，谢谢。</strong>

</div>
</div>
</article>&nbsp;

这两天用阿里云服务器重新部署网站服务器后，打开某php页面出现了如下警告：Warning: mysql_connect(): Headers and client library minor version mismatch. Headers:50547 Library:50631 in /XXX(某某目录)/wp-db.php on line 1520，虽然是警告，但是有的界面会因此打不开，甚是头疼，前不久用的是腾讯云服务器同样的部署方式并没有出现这个警告，一头雾水。

使用：
<div class="dp-highlighter bg_plain">
<div class="bar">
<div class="tools">

<b>[plain]</b> <a class="ViewSource" title="view plain" href="https://blog.csdn.net/ausboyue/article/details/52790222#">view plain</a><span data-mod="popu_168"> <a class="CopyToClipboard" title="copy" href="https://blog.csdn.net/ausboyue/article/details/52790222#">copy</a></span>
<div><embed id="ZeroClipboardMovie_1" src="https://csdnimg.cn/public/highlighter/ZeroClipboard.swf" type="application/x-shockwave-flash" width="16" height="16" align="middle" name="ZeroClipboardMovie_1" data-mce-fragment="1"></embed></div>
<div><embed id="ZeroClipboardMovie_10" src="https://csdnimg.cn/public/highlighter/ZeroClipboard.swf" type="application/x-shockwave-flash" width="16" height="16" align="middle" name="ZeroClipboardMovie_10" data-mce-fragment="1"></embed></div>
</div>
</div>
<ol start="1">
 	<li class="alt">php -i|grep Client</li>
</ol>
</div>
查询当前Client 版本，结果如下：

Client API version =&gt; 5.6.31
Client API library version =&gt; 5.6.31
Client API header version =&gt; 5.5.47-MariaDB
Client API version =&gt; 5.6.31

好吧，header version =&gt; 5.5.47-MariaDB 出现个奇葩，版本号不一样，怪不得报错了。据某大牛说，版本不兼容，需升级MariaDB版本至少到5.6.31或许可以解决。我的系统是CentOS7.2版本，众所周知，CentOS从7.x系列版本开始抛弃了MySQL，缺省安装的是MariaDB，虽然MariaDB兼容MySQL，但是我还是比较喜欢MySQL，所以我给服务器强制安装了MySQL，也希望一直用下去，对于这个情况，升级MariaDB这条路是不能走了。考虑当前安装的是php-mysql驱动，而当前的php版本比较新，想到这个奇葩是不是因为驱动版本较低造成的，于是尝试以下操作：
<div class="dp-highlighter bg_plain">
<div class="bar">
<div class="tools">

<b>[plain]</b> <a class="ViewSource" title="view plain" href="https://blog.csdn.net/ausboyue/article/details/52790222#">view plain</a><span data-mod="popu_168"> <a class="CopyToClipboard" title="copy" href="https://blog.csdn.net/ausboyue/article/details/52790222#">copy</a></span>
<div><embed id="ZeroClipboardMovie_2" src="https://csdnimg.cn/public/highlighter/ZeroClipboard.swf" type="application/x-shockwave-flash" width="16" height="16" align="middle" name="ZeroClipboardMovie_2" data-mce-fragment="1"></embed></div>
<div><embed id="ZeroClipboardMovie_11" src="https://csdnimg.cn/public/highlighter/ZeroClipboard.swf" type="application/x-shockwave-flash" width="16" height="16" align="middle" name="ZeroClipboardMovie_11" data-mce-fragment="1"></embed></div>
</div>
</div>
<ol start="1">
 	<li class="alt">yum remove php-mysql</li>
 	<li class="">yum install php-mysqlnd</li>
</ol>
</div>
先卸载较低版本的 php-mysql驱动，再升级安装新版的php-mysqlnd驱动。

&nbsp;

php-fpm 下安装命令为
<blockquote>yum install php70w-mysqlnd</blockquote>
OK,再重启下httpd和mysql服务：
<div class="dp-highlighter bg_plain">
<div class="bar">
<div class="tools">

<b>[plain]</b> <a class="ViewSource" title="view plain" href="https://blog.csdn.net/ausboyue/article/details/52790222#">view plain</a><span data-mod="popu_168"> <a class="CopyToClipboard" title="copy" href="https://blog.csdn.net/ausboyue/article/details/52790222#">copy</a></span>
<div><embed id="ZeroClipboardMovie_3" src="https://csdnimg.cn/public/highlighter/ZeroClipboard.swf" type="application/x-shockwave-flash" width="16" height="16" align="middle" name="ZeroClipboardMovie_3" data-mce-fragment="1"></embed></div>
<div><embed id="ZeroClipboardMovie_12" src="https://csdnimg.cn/public/highlighter/ZeroClipboard.swf" type="application/x-shockwave-flash" width="16" height="16" align="middle" name="ZeroClipboardMovie_12" data-mce-fragment="1"></embed></div>
</div>
</div>
<ol start="1">
 	<li class="alt">systemctl  restart httpd</li>
 	<li class="">systemctl  restart mysqld</li>
</ol>
</div>
再打开网站，能正常打开，感觉可以了，再检测下那个奇葩：
<div class="dp-highlighter bg_plain">
<div class="bar">
<div class="tools">

<b>[plain]</b> <a class="ViewSource" title="view plain" href="https://blog.csdn.net/ausboyue/article/details/52790222#">view plain</a><span data-mod="popu_168"> <a class="CopyToClipboard" title="copy" href="https://blog.csdn.net/ausboyue/article/details/52790222#">copy</a></span>
<div><embed id="ZeroClipboardMovie_4" src="https://csdnimg.cn/public/highlighter/ZeroClipboard.swf" type="application/x-shockwave-flash" width="16" height="16" align="middle" name="ZeroClipboardMovie_4" data-mce-fragment="1"></embed></div>
<div><embed id="ZeroClipboardMovie_13" src="https://csdnimg.cn/public/highlighter/ZeroClipboard.swf" type="application/x-shockwave-flash" width="16" height="16" align="middle" name="ZeroClipboardMovie_13" data-mce-fragment="1"></embed></div>
</div>
</div>
<ol start="1">
 	<li class="alt">php -i|grep Client</li>
</ol>
</div>
结果如下：

Client API version =&gt; mysqlnd 5.0.10 - 20111026
Client API library version =&gt; mysqlnd 5.0.10 - 20111026
Client API version =&gt; mysqlnd 5.0.10 - 20111026

OK，版本全统一了，问题解决。