---
ID: 814
post_title: '如何将  ecshop 的图片 部署到 CDN 服务器  ?'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/05/11/ecshop-images-to-cdn-server/
published: true
post_date: 2017-05-11 13:05:43
---
如何将  ecshop 的图片 部署到 CDN 服务器  ?

&nbsp;

chris 想给ecshop定义个图片服务器常量，在模板中的图片地址前面加上 CDN 服务器的地址。    该如何实现呢？

先看效果

<a href="http://www.kinankvm.com">http://www.kinankvm.com</a>
Kinan is one of the world’s leading suppliers of KVM Switch products and solutions; our product range encompasses KVM Switch products from the simple analog KVM to the latest advanced digital KVM.

cdn 服务器 的地址为          http://cdn.kinankvm.com

Chris 这里简单提供下思路：

1. 比如data/config.php 这里文件
我们看到
<blockquote>define('EC_CHARSET','utf-8');
define('ADMIN_PATH','admin');
define('AUTH_KEY', 'this is a key');
define('OLD_AUTH_KEY', '');</blockquote>
这些代码， define() 函数定义一个常量. 这里就采取define定义常量，然后引入系统当中作为标签使用。
比如我们想定义个标识：  define('CDN_IMG_SERVER, 'http://cdn.kinankvm.com/');

&nbsp;

&nbsp;

=============================================================

2. 然后修改includes\\lib_main.php    1625行 function assign_template   函数定义
在1644行 代码：
<blockquote>$smarty-&gt;assign('ecs_version',   VERSION);</blockquote>
下增加：
<blockquote>$smarty-&gt;assign('CDN_IMG_SERVER',   CDN_IMG_SERVER);</blockquote>
&nbsp;

=============================================================

3. 打开 themes\yourtheme\goods.dwt   或者     catagory.dwt   等文件

在图片的 src 上面加上  {$CDN_IMG_SERVER}

例如将
<blockquote>
&lt;img src="{$goods.goods_img}" id="main_img" class="main_img" style="width:100%; height:auto;" alt="{$goods.goods_name}" /&gt;&lt;/a&gt;</blockquote>
改为：
<blockquote>
&lt;img src="{$CDN_IMG_SERVER}{$goods.goods_img}" id="main_img" class="main_img" style="width:100%; height:auto;" alt="{$goods.goods_name}" /&gt;&lt;/a&gt;</blockquote>
&nbsp;

css, js  文件也可以使用CDN， 在文件路径上加上    {$CDN_IMG_SERVER} 即可。