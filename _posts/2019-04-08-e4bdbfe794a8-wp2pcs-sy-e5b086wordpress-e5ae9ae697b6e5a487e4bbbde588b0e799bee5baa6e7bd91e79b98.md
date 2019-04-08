---
ID: 2669
post_title: >
  使用 WP2PCS-SY 将wordpress
  定时备份到百度网盘
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2019/04/08/%e4%bd%bf%e7%94%a8-wp2pcs-sy-%e5%b0%86wordpress-%e5%ae%9a%e6%97%b6%e5%a4%87%e4%bb%bd%e5%88%b0%e7%99%be%e5%ba%a6%e7%bd%91%e7%9b%98/'
published: true
post_date: 2019-04-08 18:01:34
---
<div id="tab-description" class="plugin-description section">
<h2 id="description-header">Description</h2>
WP2PCS-SY是基于WP2PCS插件修改而来，在原版本的基础上取消了外链，另增加了新的功能，并做了完善， 主要功能就是把WordPress和网盘（PCS，个人云存储）连接在一起的插件。它的两项基本功能就是：将wordpress的数据库、文件<strong>备份</strong>到网盘，以防止由于过失而丢失了网站数据；把网盘作为网站的后备箱，<strong>存放</strong>图片、附件，解决网站空间不够用的烦恼，这个时候，你可以在网站内<strong>直接引用</strong>网盘上的文件，并提高你的网站SEO和用户体验，并具有防盗链功能。
WP2PCS-SY将你的WordPress定时备份到百度网盘，把百度网盘作为附件存储空间，解决你的网站后顾之忧。

WP2PCS官方网站 http://www.wp2pcs.com
WP2PCS-SY官方网站：http://www.syncy.cn

<strong>修改内容如下：</strong>
1、修改了授权模式，采用自有APIkey的时候不会再向第三方网站传输APIkey和securtkey，直接和百度服务器通信，减少了泄露Securtkey的风险；使用wp2pcs-sy的APIkey的话刷新码也存储在本地（wp2pcs-sy承诺永不存储用户的refreshtoken和accesstoken），并定期刷新accesstoken，不用再手动刷新accesstoken；
2、在多媒体下面增加了一个百度网盘的菜单，可以浏览百度网盘中上传目录下的文件，不用再到编辑文章页面才可以浏览到图片等，同时也可以通过此页面上传单个文件；
3、多媒体-百度网盘和编辑文章插入图片的页面显示的图片全部是图片的缩略图（原版本中获取的是完整图片文件），加快了图片的浏览；
4、增加了浏览网盘文件时的排序功能，可按修改时间倒排或顺排、按文件名倒排或顺排；
5、增加了文件名对特殊字符及空格的支持，文件名可支持除PCS规定不能使用的字符外的所有字符；
6、取消了外链功能，采用直链也不存在泄露accesstoken的风险；
7、增加了普通文件、mp3、通用媒体文件的缓存功能；
8、增加了对缓存文件清理的功能；
9、增加了防盗链功能；
10、优化了数据库备份功能，原版本中在BAE上备份很难成功，优化后很少失败；
11、修复了原版本中下载文件失败的bug；
12、优化了在加载下一页图片时失败，导致下一页按钮不可见的问题；
13、所有功能免费开放。

<strong>不适用范围</strong>
<ul>
 	<li>超大型网站（打包后超过G）</li>
 	<li>开启MULTISITE的多站点网站</li>
 	<li>网站空间剩余不足三分之一</li>
 	<li>没有读写权限或读写权限受限制的空间（如BAE、SAE不能备份网站文件）</li>
 	<li>服务器memory limit, time limit比较小，又不能自己修改的</li>
 	<li>主机PHP不支持ZipArchive类</li>
 	<li>免费主机、海外主机等性能差或与PCS通信弱的主机</li>
</ul>
</div>
<div id="screenshots" class="plugin-screenshots" data-reactroot="">
<h2>Screenshots</h2>
<section class="image-gallery">
<div class="image-gallery-content">
<div class="image-gallery-slides">
<div class="image-gallery-slide center">
<figure class="image-gallery-image"><img class="alignnone size-full wp-image-2670" src="http://hss5.com/wp-content/uploads/2019/04/screenshot-1.jpg" width="1280" height="795" alt="" /></figure>
</div>
<div class="image-gallery-slide right">
<figure class="image-gallery-image"><img class="alignnone size-full wp-image-2671" src="http://hss5.com/wp-content/uploads/2019/04/screenshot-2.jpg" width="548" height="277" alt="" /></figure>
</div>
<div class="image-gallery-slide">
<figure class="image-gallery-image"><img class="alignnone size-full wp-image-2672" src="http://hss5.com/wp-content/uploads/2019/04/screenshot-3.jpg" width="523" height="252" alt="" /></figure>
</div>
<div class="image-gallery-slide">
<figure class="image-gallery-image"><img class="alignnone size-full wp-image-2673" src="http://hss5.com/wp-content/uploads/2019/04/screenshot-4.jpg" width="997" height="485" alt="" /></figure>
</div>
<div class="image-gallery-slide left">
<figure class="image-gallery-image"><img class="alignnone size-full wp-image-2674" src="http://hss5.com/wp-content/uploads/2019/04/screenshot-5.jpg" width="823" height="253" alt="" /></figure>
</div>
</div>
</div>
<div class="image-gallery-thumbnails">
<div class="image-gallery-thumbnails-container"><button class="button-link image-gallery-thumbnail active" type="button"></button><button class="button-link image-gallery-thumbnail active" type="button"><img class="alignnone size-full wp-image-2675" src="http://hss5.com/wp-content/uploads/2019/04/screenshot-1-1.jpg" width="1280" height="795" alt="" /></button>
<div class="image-gallery-thumbnail-label"></div>
<button class="button-link image-gallery-thumbnail" type="button"></button><button class="button-link image-gallery-thumbnail" type="button"><img class="alignnone size-full wp-image-2676" src="http://hss5.com/wp-content/uploads/2019/04/screenshot-2-1.jpg" width="548" height="277" alt="" /></button>
<div class="image-gallery-thumbnail-label"></div>
<button class="button-link image-gallery-thumbnail" type="button"></button><button class="button-link image-gallery-thumbnail" type="button"><img class="alignnone size-full wp-image-2677" src="http://hss5.com/wp-content/uploads/2019/04/screenshot-3-1.jpg" width="523" height="252" alt="" /></button>
<div class="image-gallery-thumbnail-label"></div>
<button class="button-link image-gallery-thumbnail" type="button"></button><button class="button-link image-gallery-thumbnail" type="button"><img class="alignnone size-full wp-image-2678" src="http://hss5.com/wp-content/uploads/2019/04/screenshot-4-1.jpg" width="997" height="485" alt="" /></button>
<div class="image-gallery-thumbnail-label"></div>
<button class="button-link image-gallery-thumbnail" type="button"></button><button class="button-link image-gallery-thumbnail" type="button"><img class="alignnone size-full wp-image-2679" src="http://hss5.com/wp-content/uploads/2019/04/screenshot-5-1.jpg" width="823" height="253" alt="" /></button>
<div class="image-gallery-thumbnail-label"></div>
</div>
</div>
</section></div>
<div id="faq" class="plugin-faq section">
<h2 id="faq-header">FAQ</h2>
<ul>
 	<li>1.当发现插件出错时，开启调试获取错误信息。当插件运行缓慢或通信不良时，开启“简易加速”。</li>
 	<li>2.定时备份，在规定的时间，将网站打包备份到网盘；立即压缩下载备份的压缩文件。</li>
 	<li>3.增量备份只备份你修改或上传的文件，没有变化的文件不上传。</li>
 	<li>4.设置附件访问的前缀，通过特定的URL就可以访问网盘中的附件资源。</li>
 	<li>5.在文章编辑页面“添加媒体”按钮后的媒体管理面板中选择插入网盘中的附件。</li>
</ul>
</div>