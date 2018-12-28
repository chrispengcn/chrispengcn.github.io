---
ID: 227
post_title: zen cart 数据库缓存简介
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/08/31/zen-cart-%e6%95%b0%e6%8d%ae%e5%ba%93%e7%bc%93%e5%ad%98%e7%ae%80%e4%bb%8b/'
published: true
post_date: 2013-08-31 08:13:05
---
<h5></h5>
<a href="http://www.zen-cart.cn/forum/post39324.html#p39324"><img title="帖子" alt="帖子" src="http://www.zen-cart.cn/forum/styles/prosilver/imageset/icon_post_target.gif" width="11" height="9" /></a>由 <strong><a href="http://www.zen-cart.cn/forum/member/Jack/">Jack</a></strong> » 2009-10-08 8:04

首先，zencart的缓存指的是SQL数据库缓存，就是zencart读取数据库时，可以保存部分查询结果，一定程度上减少对数据库的查询次数。
zencart的SQL缓存设置有三个选项： none, database 和 file
前台的数据库缓存，在 \includes\configure.php 文件中设置；
后台的数据库缓存，在 \admin\includes\configure.php 文件中设置；
需要修改以下两个参数：

<dl><dt>代码: <a href="http://www.zen-cart.cn/forum/#">全选</a></dt><dd><code>  define('SQL_CACHE_METHOD', 'none');</code></dd><dd><code>
define('DIR_FS_SQL_CACHE', '/var/www/html/cache');</code></dd></dl>其中，SQL_CACHE_METHOD 即为缓存方式：
none: 无，即不使用缓存。如果您的商品和分类很少，该方式实际上速度最快。
database: 数据库，即使用数据库缓存方式。SQL查询结果缓存在数据库表中。听起来很奇怪，但对于商品和分类数量中等的网站来说，可以加快速度。
file: 文件，即使用文件缓存方式。SQL查询结果缓存于服务器的硬盘上。该方式适合有大量的商品和分类的网站。
如果选择 file 方式，需要同时设置 DIR_FS_SQL_CACHE 为缓存文件的目录，这也是商店管理员需要定时删除缓存文件的目录。建议使用zencart的cache目录，该目录必须可写 (chmod 666 或者 chmod 777)。