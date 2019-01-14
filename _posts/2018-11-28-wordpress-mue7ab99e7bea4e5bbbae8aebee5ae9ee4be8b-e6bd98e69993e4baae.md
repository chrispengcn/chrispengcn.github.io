---
ID: 1492
post_title: >
  使用WordPress
  MU一个程序创建多站点网络介绍与安装教程
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/28/wordpress-mu%e7%ab%99%e7%be%a4%e5%bb%ba%e8%ae%be%e5%ae%9e%e4%be%8b-%e6%bd%98%e6%99%93%e4%ba%ae/'
published: true
post_date: 2018-11-28 15:25:44
---
<header class="post-head">
<h1>使用WordPress MU一个程序创建多站点网络介绍与安装教程</h1>
</header>
<div class="content-post">

<img class="alignnone size-full wp-image-1500" src="http://hss5.com/wp-content/uploads/2018/11/mu_000.jpg" width="800" height="450" alt="创建WordPress Multisite Network多站点网络安装教程" />
经常折腾wp的人肯定不止一个博客，主题高产的WPER那就更不用说，所以我们不想重复地安装wordpress，通过WordPress MU只需要安装一次就可以无限地创建WP站点。

WordPress MU是WordPress多博客的版本，而程序本身都是一样的。

<a href="https://salongweb.com/go?url=https://yfdxs.com/sa-longlong" target="_blank" rel="nofollow noopener">萨龙龙</a>也有很多个站点，博客、<a href="https://salongweb.com/" target="_blank" rel="noopener">萨龙网络</a>、演示站和<a href="https://salongweb.com/go?url=https://taji.me" target="_blank" rel="nofollow noopener">它季|专属民族格调商城！</a>等等，尤其是商城做成简、繁、英三种语言的就更需要使用MU来搭建，每个站点可以单独设置语言，互不干扰。很早就测试过MU的安装与配置，这次花了几个小时的时间把所有的站点（除了商城）都安装在一个MU中。
<div class="related_posts">
<h2 class="icon-th-list"><a title="查看“WordPress多站点”标签的相关文章列表" href="https://salongweb.com/tag/wordpress%e5%a4%9a%e7%ab%99%e7%82%b9">“WordPress多站点”的相关文章</a></h2>
<ul>
 	<li><a title="使用WordPress MU一个程序创建多站点网络介绍与安装教程" href="https://salongweb.com/wordpress-multisite-network.html">使用WordPress MU一个程序创建多站点网络介绍与安装教程</a></li>
 	<li><a title="如何把WordPress数据迁移至WordPress MU" href="https://salongweb.com/wordpress-to-wordpress-mu.html">如何把WordPress数据迁移至WordPress MU</a></li>
 	<li><a title="WordPress MU多站点网络域名绑定插件Domain Mapping的安装与使用" href="https://salongweb.com/wordpress-mu-domain-mapping.html">WordPress MU多站点网络域名绑定插件Domain Mapping的安装与使用</a></li>
 	<li><a title="WordPress MU多站点网络共享媒体插件Network Shared Media的使用包括特色图像" href="https://salongweb.com/wp-network-shared-media-featured-image-support.html">WordPress MU多站点网络共享媒体插件Network Shared Media的使用包括特色图像</a></li>
 	<li><a title="WordPress MU多站点网络克隆新站点插件Multisite Cloner" href="https://salongweb.com/wordpress-mu-multisite-cloner.html">WordPress MU多站点网络克隆新站点插件Multisite Cloner</a></li>
 	<li><a title="WordPress MU多站点从子域名转子目录" href="https://salongweb.com/wordpress-mu-subdomain-subdirectory.html">WordPress MU多站点从子域名转子目录</a></li>
 	<li><a title="WordPress MU多站点设置子站点上传路径和文件的URL地址" href="https://salongweb.com/wordpress-mu-media-path.html">WordPress MU多站点设置子站点上传路径和文件的URL地址</a></li>
 	<li><a title="WordPress MU多站点解决Timthumb.php不显示缩略图" href="https://salongweb.com/wordpress-mu-timthumb.html">WordPress MU多站点解决Timthumb.php不显示缩略图</a></li>
</ul>
</div>
<strong>安装前需要需确认主机支持伪静态！</strong>

<strong>经过在虚拟主机上测试，MU只能安装在主域名下，子域名可以安装，但创建的站点不能访问；同时在子域名下安装时，选择“子目录”安装，完成后登录不了后台。有朋友测试安装没有问题可以留言告之，谢谢。本地使用wamp环境“子目录”安装MU是没有问题，一切都正常，所以还是表明MU安装在主目录下是没有问题。</strong>[yuanfangbox]MU的安装[/yuanfangbox]<strong>一、添加开启MU代码</strong>

WordPress本身已经集成了MU，默认是关闭的，所以不需要安装任何程序，开启只需添加一段代码。找到网站根目录下的wp-config.php文件，在
<div class="dp-highlighter">
<ol class="dp-c">
 	<li class="alt"><span class="comment">/* 好了！请不要再继续编辑。请保存本文件。使用愉快！ */</span></li>
</ol>
</div>
上面添加
<div class="dp-highlighter">
<ol class="dp-c">
 	<li class="alt">define('WP_ALLOW_MULTISITE', true);</li>
</ol>
</div>
<strong>二、安装MU</strong>

进行下边的操作之前一定要先禁用全部的插件。

然后把wp-config.php文件上传至网站根目录，刷新后在网站后台——工具就有“配置网络”菜单，进入如下图：
<img class="alignnone size-full wp-image-1501" src="http://hss5.com/wp-content/uploads/2018/11/mu_001.jpg" width="800" height="789" alt="创建WordPress Multisite Network多站点网络安装教程" />

选择站点的链接地址方式，可以选择“子域名”或“子目录”两种方式，链接方式如图中的链接。在本地或子目录下安装不会提示选择链接方式，因为只能以“子目录”方式安装。如果使用“子域名”方式安装，需要主机支持泛域名解析，同时添加DNS记录。

<strong>三、添加网络所需的代码</strong>

点击安装后，进行下一步之前建议备份当前的wp-config.php文件和.htaccess文件，然后把
<div class="dp-highlighter">
<ol class="dp-c">
 	<li class="alt">define('MULTISITE', true);</li>
 	<li>define('SUBDOMAIN_INSTALL', false);</li>
 	<li class="alt">define('DOMAIN_CURRENT_SITE', 'sixianqiu.com');</li>
 	<li>define('PATH_CURRENT_SITE', '/');</li>
 	<li class="alt">define('SITE_ID_CURRENT_SITE', 1);</li>
 	<li>define('BLOG_ID_CURRENT_SITE', 1);</li>
</ol>
</div>
添加到wp-config.php文件的/* 好了！请不要再继续编辑。请保存本文件。使用愉快！ */的上方，然后把
<div class="dp-highlighter">
<ol class="dp-c">
 	<li class="alt">RewriteEngine On</li>
 	<li>RewriteBase /</li>
 	<li class="alt">RewriteRule ^index.php$ - [L]</li>
 	<li></li>
 	<li class="alt"># add a trailing slash to /wp-admin</li>
 	<li>RewriteRule ^([_0-9a-zA-Z-]+/)?wp-admin$ <span class="vars">$1wp</span>-admin/ [R=301,L]</li>
 	<li class="alt"></li>
 	<li>RewriteCond %{REQUEST_FILENAME} -f [OR]</li>
 	<li class="alt">RewriteCond %{REQUEST_FILENAME} -d</li>
 	<li>RewriteRule ^ - [L]</li>
 	<li class="alt">RewriteRule ^([_0-9a-zA-Z-]+/)?(wp-(content|admin|includes).*) <span class="vars">$2</span> [L]</li>
 	<li>RewriteRule ^([_0-9a-zA-Z-]+/)?(.*.php)$ <span class="vars">$2</span> [L]</li>
 	<li class="alt">RewriteRule . index.php [L]</li>
</ol>
</div>
添加到.htaccess文件中，并覆盖掉其他WordPress规则，完成后上传wp-config.php文件和.htaccess文件到网站根目录下。

上传完成后点击页面正文的“登录”按钮重新登录MU，进入后台就能看到MU仪表盘：
<img class="alignnone size-full wp-image-1502" src="http://hss5.com/wp-content/uploads/2018/11/mu_002.jpg" width="800" height="450" alt="创建WordPress Multisite Network多站点网络安装教程" />

<strong>四、创建站点</strong>

进入管理网络——站点中，创建站点，输入相关信息，点击“添加站点”，一个新的WP站点就创建了
<img class="alignnone size-full wp-image-1503" src="http://hss5.com/wp-content/uploads/2018/11/mu_003.jpg" width="800" height="450" alt="创建WordPress Multisite Network多站点网络安装教程" />

新添加的站点都会显示在“我的站点”列表下，我们可以点击进入新站点的仪表盘、写文章等等
<img class="alignnone size-full wp-image-1504" src="http://hss5.com/wp-content/uploads/2018/11/mu_004.jpg" width="800" height="450" alt="创建WordPress Multisite Network多站点网络安装教程" />

在MU中创建的站点功能与独立的wordpress站点一样，只是不能安装、编辑主题和插件，这些都需要在“管理网络”中进行。
<div class="infobox">萨龙龙写的文章都是通过在实践中而总结的，远方的雪山就是通过MU来创建的，接下来会写一系列MU方面的教程，包括创建站点、数据的迁移、MU相关插件使用等教程。</div>
</div>