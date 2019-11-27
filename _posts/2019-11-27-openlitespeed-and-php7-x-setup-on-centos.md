---
ID: 3351
post_title: OpenLiteSpeed and php7.x setup on centos
author: peng, chris
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/11/27/openlitespeed-and-php7-x-setup-on-centos/
published: true
post_date: 2019-11-27 14:21:45
---
OpenLiteSpeed 是 LiteSpeed Technologies 开发的开源HTTP服务器。OpenLiteSpeed 具有高性能和轻量级的特点，并带有一个 Web GUI 管理界面，可以处理超过十万个具有低资源使用（CPU 和 RAM）的并发连接。OpenLiteSpeed 支持许多操作系统，如 Linux，Mac OS，FreeBSD 和 SunOS，可用于运行用 PHP，Ruby Perl 和 java 编写的网站脚本。

在本篇教程中，我将指导您在<a href="https://cloud.tencent.com/product/cvm?from=10680" target="_blank" rel="noopener noreferrer" data-text-link="2_1358153" data-from="10680">云服务器</a>上安装并配置 OpenLiteSpeed 和 PHP 7（我们将用 CentOS 作为演示版本）。 如果您还没有腾讯云的服务器，可以先<a href="https://cloud.tencent.com/act/free?from=10680" target="_blank" rel="noopener noreferrer" data-from="10680">点击这里</a>进行免费套餐的试用。免费套餐包含企业版和个人版，超过11款热门产品和42款长期免费的云产品可以供您选择。如果您有长期搭建服务器的需求的话，可以<a href="https://cloud.tencent.com/product/cvm?from=10680" target="_blank" rel="noopener noreferrer" data-from="10680">点击这里</a>进行服务器的购买，现在的促销力度很大哦。
<h2 id="%E6%B7%BB%E5%8A%A0-OpenLiteSpeed-%E5%AD%98%E5%82%A8%E5%BA%93"><strong>添加 OpenLiteSpeed 存储库</strong></h2>
要在CentOS服务器上安装 OpenLiteSpeed，我们必须添加 lite 速度存储库。使用此 rpm 命令添加它：
<pre class="prism-token token  language-javascript">rpm <span class="token operator">-</span>ivh http<span class="token punctuation">:</span><span class="token operator">/</span><span class="token operator">/</span>rpms<span class="token punctuation">.</span>litespeedtech<span class="token punctuation">.</span>com<span class="token operator">/</span>centos<span class="token operator">/</span>litespeed<span class="token operator">-</span>repo<span class="token number">-1.1</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">.</span>el7<span class="token punctuation">.</span>noarch<span class="token punctuation">.</span>rpm</pre>
<h2 id="%E5%AE%89%E8%A3%85-OpenLiteSpeed"><strong>安装 OpenLiteSpeed</strong></h2>
在此步骤中，我们将安装 OpenLiteSpeed 1.4。这是稳定版本，具有 Pagespeed，文件上传，PHP 7 支持，RCS 集成和 HTTP / 2支持等许多功能。让我们使用以下yum 命令安装 OpenLiteSpeed 1.4：
<pre class="prism-token token  language-javascript">yum <span class="token operator">-</span>y install openlitespeed14<span class="token punctuation">.</span>x86_64</pre>
等待安装完成。
<h2 id="%E5%AE%89%E8%A3%85-Epel-%E5%AD%98%E5%82%A8%E5%BA%93%E5%92%8C-PHP-7"><strong>安装 Epel 存储库和 PHP 7</strong></h2>
PHP 7安装需要 Epel 存储库。它可以在 CentOS 存储库中使用。使用 yum 命令安装 Epel 存储库：
<pre class="prism-token token  language-javascript">yum <span class="token operator">-</span>y install epel<span class="token operator">-</span>release</pre>
接下来，为openLiteSpeed安装php 7。OpenLiteSpeed 使用特殊版本的 PHP，OpenLiteSpeed 的 PHP 版本以“ls”开头。通过键入以下内容安装带有多扩展的 PHP 7 以获得丰富的功能集：
<pre class="prism-token token  language-javascript">yum <span class="token operator">-</span>y install lsphp70 lsphp70<span class="token operator">-</span>mysqlnd lsphp70<span class="token operator">-</span>process lsphp70<span class="token operator">-</span>mbstring lsphp70<span class="token operator">-</span>mcrypt lsphp70<span class="token operator">-</span>gd lsphp70<span class="token operator">-</span>opcache lsphp70<span class="token operator">-</span>bcmath lsphp70<span class="token operator">-</span>pdo lsphp70<span class="token operator">-</span>common lsphp70<span class="token operator">-</span>xml</pre>
如果要查看所有 PHP 扩展的列表，可以使用 yum search 命令：
<pre class="prism-token token  language-javascript">yum search lsphp70</pre>
<h2 id="%E9%85%8D%E7%BD%AE-OpenLiteSpeed-%E5%92%8C-PHP-7"><strong>配置 OpenLiteSpeed 和 PHP 7</strong></h2>
在这一步中，我们将配置 OpenLiteSpeed 和 PHP 7。OpenLiteSpeed 有一个管理界面，因此我们将配置 OpenLiteSpeed GUI 的管理员密码，然后配置 PHP 7 以使用 OpenLiteSpeed 并打开标准HTTP端口<code>80</code>。
<h3 id="%E9%85%8D%E7%BD%AE%E5%B9%B6%E6%B5%8B%E8%AF%95-GUI">配置并测试 GUI</h3>
要配置GUI的管理员用户和密码，请运行以下命令：
<pre class="prism-token token  language-javascript"><span class="token operator">/</span>usr<span class="token operator">/</span>local<span class="token operator">/</span>lsws<span class="token operator">/</span>admin<span class="token operator">/</span>misc<span class="token operator">/</span>admpass<span class="token punctuation">.</span>sh</pre>
输入GUI 管理的用户名和密码。
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3352" src="https://www.hss5.com/wp-content/uploads/2019/11/kcqwksb18o.png" width="517" height="247" /></div></figure>
接下来，打开 Web 浏览器并使用端口7080访问服务器 IP 地址。
<pre class="prism-token token  language-javascript">https<span class="token punctuation">:</span><span class="token operator">/</span><span class="token operator">/</span><span class="token number">192.168</span><span class="token punctuation">.</span><span class="token number">1.108</span><span class="token punctuation">:</span><span class="token number">7080</span><span class="token operator">/</span></pre>
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3353" src="https://www.hss5.com/wp-content/uploads/2019/11/i4nc2ftehm.png" width="550" height="362" /></div></figure>
输入您的用户名和密码，然后按“登录”，您将看到 OpenLiteSpeed 仪表板。
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3354" src="https://www.hss5.com/wp-content/uploads/2019/11/4umu6yv0ks.png" width="550" height="317" /></div></figure>
<h3 id="%E9%85%8D%E7%BD%AE-PHP-7">配置 PHP 7</h3>
默认情况下，OpenLiteSpeed 1.4 使用 PHP 5，在此步骤中，我们将其更改为 PHP 7。

Php 7安装在服务器上，我们只需要通过浏览器中的管理GUI添加新配置。

单击“服务器配置”，然后单击“外部应用程序”选项卡。你会看到带有套接字地址的“lsphp5”。单击右侧的“添加”按钮添加新的“lsphp70”。
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3355" src="https://www.hss5.com/wp-content/uploads/2019/11/onusvqdaf3.png" width="550" height="156" /></div></figure>
对于该类型，请使用“LiteSpeed SAPI App”并单击“下一步”
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3356" src="https://www.hss5.com/wp-content/uploads/2019/11/6csk2sl1ps.png" width="550" height="97" /></div></figure>
接下来，添加以下配置：
<pre class="prism-token token  language-javascript">Name<span class="token punctuation">:</span> lsphp70
Address<span class="token punctuation">:</span>    uds<span class="token punctuation">:</span><span class="token operator">/</span><span class="token operator">/</span>tmp<span class="token operator">/</span>lshttpd<span class="token operator">/</span>lsphp<span class="token punctuation">.</span>sock
Max Connections<span class="token punctuation">:</span> <span class="token number">35</span>
Environment<span class="token punctuation">:</span> PHP_LSAPI_MAX_REQUESTS<span class="token operator">=</span><span class="token number">500</span>
             PHP_LSAPI_CHILDREN<span class="token operator">=</span><span class="token number">35</span>
Initial Request <span class="token function">Timeout</span> <span class="token punctuation">(</span>secs<span class="token punctuation">)</span><span class="token punctuation">:</span> <span class="token number">60</span>
Retry Timeout <span class="token punctuation">:</span> <span class="token number">0</span>
Response Buffering<span class="token punctuation">:</span> no
Auto Start<span class="token punctuation">:</span> yes
Command<span class="token punctuation">:</span> $SERVER_ROOT<span class="token operator">/</span>lsphp70<span class="token operator">/</span>bin<span class="token operator">/</span>lsphp
Back Log<span class="token punctuation">:</span> <span class="token number">100</span>
Instances<span class="token punctuation">:</span> <span class="token number">1</span>
Memory Soft <span class="token function">Limit</span> <span class="token punctuation">(</span>bytes<span class="token punctuation">)</span><span class="token punctuation">:</span> 2047M
Memory Hard <span class="token function">Limit</span> <span class="token punctuation">(</span>bytes<span class="token punctuation">)</span><span class="token punctuation">:</span>2047M
Process Soft Limit<span class="token punctuation">:</span> <span class="token number">400</span>
Process Hard Limit<span class="token punctuation">:</span> <span class="token number">500</span></pre>
单击“保存”图标以保存配置。
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3357" src="https://www.hss5.com/wp-content/uploads/2019/11/cc3gq7yt7o.png" width="550" height="333" /></div></figure>
然后转到“脚本处理程序”选项卡并编辑“lsphp5” 5脚本处理程序。将处理程序名称更改为“lsphp70”。

单击保存图标。
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3358" src="https://www.hss5.com/wp-content/uploads/2019/11/zvcz3u125g.png" width="550" height="243" /></div></figure>
<h3 id="%E9%85%8D%E7%BD%AE%E7%AB%AF%E5%8F%A380">配置端口80</h3>
OpenLiteSpeed 的默认http端口是8080，它用于接收客户端请求。在此步骤中，我们将从 OpenLiteSpeed 管理GUI将端口更改为80。

在左侧，转到“Listeners”部分以查看侦听器配置。您将看到端口为8080的默认侦听器。单击“查看”缩放图标以查看详细信息配置。现在点击“编辑”。
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3359" src="https://www.hss5.com/wp-content/uploads/2019/11/feh6ruqwal.png" width="550" height="93" /></div></figure>
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3360" src="https://www.hss5.com/wp-content/uploads/2019/11/mjk084bm05.png" width="550" height="128" /></div></figure>
<pre class="prism-token token  language-javascript">IP Address<span class="token punctuation">:</span> ANY
Port <span class="token number">80</span></pre>
将端口更改为80并保存配置。
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3361" src="https://www.hss5.com/wp-content/uploads/2019/11/i2rb3dp71x.png" width="550" height="235" /></div></figure>
如果全部完成，请单击“重启”按钮重新启动 OpenLiteSpeed，然后单击“是”进行确认。
<h2 id="%E6%B5%8B%E8%AF%95"><strong>测试</strong></h2>
现在我们可以测试服务器了。使用端口80访问服务器IP地址以确保我们的配置正常工作：<code>http://192.168.1.108/</code>
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3362" src="https://www.hss5.com/wp-content/uploads/2019/11/zjsx6s2bkv.png" width="550" height="281" /></div></figure>
要测试PHP配置，请单击PHP信息。

<code>http://192.168.1.108/phpinfo.php</code>

所有工作正常。
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3363" src="https://www.hss5.com/wp-content/uploads/2019/11/yjiaxwlkxm.png" width="550" height="383" /></div></figure>
<h2 id="%EF%BC%88%E5%8F%AF%E9%80%89%EF%BC%89%E6%9B%B4%E6%94%B9%E9%BB%98%E8%AE%A4%E7%AE%A1%E7%90%86%E7%AB%AF%E5%8F%A3"><strong>（可选）更改默认管理端口</strong></h2>
此步骤是可选的，但我建议更改 OpenLiteSpeed 的 Admin GUI 的默认端口。

要更改默认管理端口配置，请单击“WebAdmin 设置”，然后单击“监听器”，现在单击操作以编辑默认端口。
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3364" src="https://www.hss5.com/wp-content/uploads/2019/11/r5f0y9fs61.png" width="550" height="154" /></div></figure>
单击 “编辑”图标并输入管理配置的端口，然后单击“保存”图标。
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3365" src="https://www.hss5.com/wp-content/uploads/2019/11/mk0jto84vt.png" width="550" height="138" /></div></figure>
<figure>
<div class="image-block"><img class="alignnone size-full wp-image-3366" src="https://www.hss5.com/wp-content/uploads/2019/11/i6339m3e1k.png" width="550" height="209" /></div></figure>
接下来，从浏览器重新加载 OpenLiteSpeed 并检查 Web 管理员。
<pre class="prism-token token  language-js">http<span class="token punctuation">:</span><span class="token operator">/</span><span class="token operator">/</span><span class="token number">192.168</span><span class="token punctuation">.</span><span class="token number">1.108</span><span class="token punctuation">:</span><span class="token number">8088</span><span class="token operator">/</span></pre>
<h2 id="%E7%BB%93%E8%AE%BA"><strong>结论</strong></h2>
OpenLiteSpeed 是 LiteSpeed 开发的 Linux，Windows Mac 和 BSD 的开源 HTTP 服务器。OpenLiteSpeed 使用不同的PHP版本，其名称为“lsphp”，并且支持lsphp7 或 PHP 7 。通过其管理GUI可以轻松配置 OpenLiteSpeed，因此我们可以从浏览器进行配置。若您想在实验室环境抢先体验搭建自己的网站，博客或者各类应用，我推荐您到<a href="https://cloud.tencent.com/developer/labs/gallery?category=app_build&amp;from=10680" target="_blank" rel="noopener noreferrer" data-from="10680">腾讯云实验室</a>页面进行选择，不仅有步骤指导，还可以免费上机，帮助您快速掌握开发知识！

&nbsp;

https://cloud.tencent.com/developer/article/1358153