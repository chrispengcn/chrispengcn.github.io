---
ID: 434
post_title: >
  PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/10/11/prestashop%e5%8a%a0%e9%80%9f11%e6%8b%9b%e7%ab%8b%e5%88%bb%e5%8a%a0%e9%80%9fprestashop%e5%a4%96%e8%b4%b8%e7%94%b5%e5%ad%90%e5%95%86%e5%8a%a1%e7%bd%91%e7%ab%99%e6%97%a0%e9%a2%9d%e5%a4%96prestashop/'
published: true
post_date: 2013-10-11 14:20:28
---
自从PrestaShop进入1.4时代，我们会发现PrestaShop越来越慢了！比如你用的是justhost空间总会收到服务商提示你cpu超负荷的邮件，最后你的PrestaShop VPS被关闭了！
今天，你的客户是一秒钟，离离开你的PrestaShop网上商店。像亚马逊这样的顶级零售商提醒我们的PrestaShop电子商务市场是怎样的竞争力和割喉。

一个非常有趣和强大的故事，关于它的竞争性，是杰夫·贝索斯（Amazon.com的CEO及创始人）用来满足他的PrestaShop团队，每天早上比较其竞争对手的加载速度，以确保他们至少有两个竞争对手快三倍。即时你的<a href="http://www.myusbkey.net/category/prestashop/prestashop-seo/">prestashop seo</a>很好，但是网速太慢客户也会离开的！

我不会骗你，加快您的PrestaShop网站是繁琐的技术。然而，好消息是，Prestashop的最能为你做的工作！

要取得PrestaShop加速成功，并保持它简单，按照这10个技巧：
<h5>1。检查你的<a href="http://www.myusbkey.net/?s=%E6%A8%A1%E6%9D%BF">PrestaShop模板</a>引擎Smarty的正确配置</h5>
Back Office &gt; Preferences &gt; Performance 检查“Force compile”设置为false
检查“Cache”设置为true

<a href="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/picture-1.jpg"><img title="PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" src="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/picture-1.jpg" alt="grey PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" width="560" height="221" /></a>

PrestaShop – Configure Smarty

如上图设置就启用了smarty缓存加速网站！
适合你站做好上线之后，如果你需要频繁改动网站模板的话，会产生很多prestashop smarty缓存，必须清除smarty缓存后，才可以看到网站最新效果！推荐使用<a href="http://www.myusbkey.net/prestashop%e6%b8%85%e7%a9%ba%e6%9c%8d%e5%8a%a1%e5%99%a8%e7%bc%93%e5%ad%98/">PrestaShop清空服务器缓存</a>清除smarty缓存！
<h5>2。启用PrestaShop CCC认证（压缩，合并，高速缓存）</h5>
Back Office &gt; Preferences &gt; Performance

<a href="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/picture-2.jpg"><img title="PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" src="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/picture-2.jpg" alt="grey PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" width="560" height="378" /></a>

PrestaShop – CCC
<h5>3。
Use Rijndael with mcrypt lib.
来加密您PrestaShop cookie。</h5>
Back Office &gt; Preferences &gt; Performance 选择第一个按钮“Ridjnael” 配置加密，图片中截图是你现在的默认的配置。请选择第一个“Use Rijndael with mcrypt lib.”

<a href="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/picture-3.jpg"><img title="PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" src="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/picture-3.jpg" alt="grey PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" width="560" height="171" /></a>

PrestaShop – Configure Ciphering

警告：您必须安装mcrypt的PHP扩展您的服务器上，否则你会得到一个错误信息。
<h5>4。切换到新的PrestaShop图片目录存储结构</h5>
Back Office &gt; Preferences &gt; Images 在新的Prestashop V1.4，我们提供一个新的存储体系结构的图片。其主要目标是，以避免在相同的“/img/p/”文件夹下超过10万张图片。相反，图片将被传播到子文件夹（例如：“/ img/p/1/2 /”等）。

<a href="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/picture-4.jpg"><img title="PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" src="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/picture-4.jpg" alt="grey PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" width="560" height="235" /></a>

PrestaShop – Move Images

如果你的店铺，从以前的Prestashop版本升级，你需要做到以下几点，采取这种改进的优点：
作为这个过程可能需要一段时间，确保您的服务器可以运行PHP脚本，超过30秒。如果你不知道，问您的托管服务提供商。
点击“Move images”移动图像

如果你的是PrestaShop 1.2或者PrestaShop 1.3版本的，可以使用<a href="http://www.myusbkey.net/prestashop%e4%ba%a7%e5%93%81%e5%9b%be%e7%89%87%e5%88%86%e7%a6%bb%e5%9b%be%e7%89%87%e5%9c%b0%e5%9d%80%e4%b8%8d%e5%8f%98-%e8%a7%a3%e5%86%b3%e5%9b%be%e7%89%87%e5%a4%aa%e5%a4%9a%e9%97%ae%e9%a2%98-seo/">PrestaShop产品图片分离,图片地址不变 – 解决图片太多问题 SEO好 </a>进行解决图片分离的问题.
<h5>5。设置PrestaShop子域名创建JavaScript文件和CSS文件的子域</h5>
Back Office &gt; Preferences &gt; Performance Media servers (used only with CCC)

创建一个子域js1.mystore.com，并要求您的托管服务提供商直接至/ JS /
创建一个子域js2.mystore.com，请问您的托管服务提供商直接/themes/mytheme / JS /
创建一个子域css1.mystore.com，并要求您的托管服务提供商直接至/ CSS /
创建一个子域css2.mystore.com，并要求您的托管服务提供商/themes/ mytheme / CSS /

这4个子域将让你的访问者在同一时间加载多个文件。基本上，一个网页浏览器是有限的8个并行下载。每个子域加入8个新的并行下载，所以总将是40（主域名+ 4子域）。

6。设置PrestaShop CDN,全球PrestaShop加速

Back Office &gt; Preferences &gt; Performance CloudCache

你应该放眼全球，您的网站有来自世界各地遍布全球的快速加载。这就是为什么使用CDN（内容分发网络）是最有效的减少您的服务器和你的访问者之间的距离。

Prestashop的合作与最佳的类的CDN市场，CloudCache。 CloudCache模块提供免费的Prestashop的用户在其网站上使用“PRESTA25”优惠券特别优惠。

我们的“免费模块”一节中下载免费<a href="http://addons.prestashop.com/en/content-management/5094-cloudcache.html">CloudCache模块</a>模块安装在你的店铺
创建使用的“PRESTA25”优惠券帐户CloudCache的
配置模块与您CloudCache的API密钥

大功告成！模块将确保你的照片自动同步与CDN。
<h5>7。您的所有PrestaShop小图标合并成使用一张PrestaShop图片(CSS sprites)，然后通过PrestaShop CSS来对PrestaSho 图片定位</h5>
每个您的访问者是从商店的页面加载的时间，他们的网络浏览器装载约75至100张图片。其中大多数是小装饰画，可以很容易地合并成一个大的图片称为“<a href="http://baike.baidu.com/view/2173476.htm">CSS精灵</a>”。

这里是一个来自亚马逊的样本：

<a href="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/amazon-sprite.jpg"><img title="PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" src="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/amazon-sprite.jpg" alt="grey PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" width="560" height="523" /></a>

PrestaShop – Amazon Sprite

这项技术的主要好处是：
<ul>
 	<li>更快的页面加载</li>
 	<li>更少的服务器使用（服务器将提供1个文件，而不是100）</li>
 	<li>较小的HTML代码</li>
</ul>
这里只有一个缺点，这是随着时间的推移可维护性;事实上，它可以是费时添加新图片到你的精灵（图片编辑，定位坐标等）。
<h5>8。启用Prestashop memcached的XCache</h5>
Back Office &gt; Preferences &gt; Performance Memcached是一个免费的分布式内存对象缓存系统。这是非常简单但功能强大，且易于部署。它可以解决许多问题，面临大量的数据和缓存已经被维基百科，Twitter或Craigslist的。

<a href="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/picture-5.jpg"><img title="PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" src="http://preprod-img.prestashopinc.netdna-cdn.com/blog/articles-blog/2012-04-03/picture-5.jpg" alt="grey PrestaShop加速11招立刻加速PrestaShop外贸电子商务网站无额外PrestaShop插件" width="560" height="201" /></a>

PrestaShop – MemCached

Prestashop的是已经准备使用memcached的，可以在一秒钟内启用：

请问您的托管服务提供商，使你的店的memcached
去你的管理面板，并点击“首选项”选项卡，然后“性能”，看看“缓存”一节
在“系统缓存”的下拉菜单选择memcached的作为替代，你也可以使用XCache（可用的Prestashop V1.5），或Prestashop的默认缓存从下拉菜单中，它不需要任何服务器端扩展的系统。
<h5>9。安装eAccelerator的或APC这样的操作码缓存工具加速PrestaShop</h5>
eAccelerator的是一个免费的PHP代码加速器和优化。它可以提高PHP脚本的性能，缓存在其编译的状态，使编译的开销几乎完全消除。它还优化，以加快其执行的脚本。 eAccelerator在通常减少服务器的负载和你的PHP代码的速度提高了1-10倍。

有没有需要的Prestashop内完成工作与eAccelerator的具体配置。简单地问您的托管服务提供商，使eAccelerator的，确保你的店仍然正常工作。
<h5>10。调整你的MySQL配置，并检查您的SQL查询缓存值让PrestaShop数据库缓存起效</h5>
请问您的托管服务提供商深入MySQL配置和检查的query_cache值。这个值应该是最起码的“512M”（512兆字节）。

其他MySQL的配置值，也可以进行微调，有一个优秀的MySQL性能博客看看。
<h6><a href="http://www.myusbkey.net/mysql%e6%80%a7%e8%83%bd%e5%8a%a0%e9%80%9f%e5%88%86%e6%9e%90%e4%bb%a5%e5%8f%8aphpmyadmin%e4%b8%adexplain%e7%94%a8%e6%b3%95/">MySql性能加速分析以及PHPMYADMIN中explain用法 </a></h6>
<h5>11.进入你的ftp，删除你不需要的modules</h5>
这样可以加快modules加载时间

&nbsp;

<a title="http://www.myusbkey.net/" href="http://www.myusbkey.net/">http://www.myusbkey.net/</a>