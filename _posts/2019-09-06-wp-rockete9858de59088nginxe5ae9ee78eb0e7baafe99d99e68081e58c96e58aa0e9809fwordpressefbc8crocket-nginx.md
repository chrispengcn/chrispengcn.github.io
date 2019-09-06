---
ID: 3241
post_title: >
  wp-rocket配合nginx实现纯静态化加速wordpress，Rocket-Nginx
author: peng, chris
post_excerpt: ""
layout: post
permalink: 'https://www.hss5.com/2019/09/06/wp-rocket%e9%85%8d%e5%90%88nginx%e5%ae%9e%e7%8e%b0%e7%ba%af%e9%9d%99%e6%80%81%e5%8c%96%e5%8a%a0%e9%80%9fwordpress%ef%bc%8crocket-nginx/'
published: true
post_date: 2019-09-06 17:00:26
---
WP Rocket缓存插件是当前最高效也是最灵活的<a title="查看与 WordPress 相关的文章" href="https://www.wanvi.net/tag/wordpress/" target="_blank" rel="noopener noreferrer">WordPress</a>静态缓存插件。WP Rocket在性能方面集成了所有最新功能：延迟图像加载，延迟加载javascipt，缩小html代码体积，连接和所辖javascript文件。WP Rocket还拥有自己的自托管爬虫机器人，它将访问您的站点并生成缓存文件，以便当人访问者访问您的站点时，他们会立即获得该页面的快速缓存版本。我们还有一个站点地图预载功能。

<img class="alignnone size-full wp-image-3242" src="https://www.hss5.com/wp-content/uploads/2019/09/2019030215250236.png" width="1280" height="582" alt="wp-rocket配合nginx实现纯静态化加速wordpress，Rocket-Nginx" />

然而它还是有一点毛病，那就是它依然是通过wordpress的php运行来提供缓存，不能算是真正的静态加载了，我们能不能跳过php执行的步骤，直接引导加载缓存文件呢？答案是可以的。
<h2>关闭wordpress的cron定时任务</h2>
你可能已经知道wordpress的cron定时任务并不是真正的定时任务，只有访问网站是才会执行定时任务，这个是不是有点假。为了确保cron计划任务在应用时运行，强烈建议禁用<a title="查看与 WordPress 相关的文章" href="https://www.wanvi.net/tag/wordpress/" target="_blank" rel="noopener noreferrer">WordPress</a> cron作业并创建真正的cron作业。

要禁用WordPress cron作业，请将以下行添加到<code>wp-config.php</code>：
<div class="dp-highlighter">
<div class="bar"></div>
<ol class="dp-j" start="1">
 	<li class="alt">define('DISABLE_WP_CRON', <span class="keyword">true</span>);</li>
</ol>
</div>
然后我们手动常见一个定时任务，支持get、curl、php等几种方式触发任务。

我们设置定时任务每15分钟执行一次就可以了
<div class="dp-highlighter">
<div class="bar"></div>
<ol class="dp-j" start="1">
 	<li class="alt">*/<span class="number">15</span> * * * * wget -q -O - http:<span class="comment">//www.website.com/wp-cron.php?doing_wp_cron &amp;&gt;/dev/null</span></li>
 	<li class="">*/<span class="number">15</span> * * * * curl http:<span class="comment">//www.website.com/wp-cron.php?doing_wp_cron &amp;&gt;/dev/null</span></li>
 	<li class="alt">*/<span class="number">15</span> * * * * cd /home/user/public_html; php wp-cron.php &amp;&gt;/dev/<span class="keyword">null</span></li>
</ol>
</div>
使用虚拟机的小伙伴可以使用使用第三方任务监控,例如360云监控等。
<h2>编译并安装rocket-nginx</h2>
要使用该脚本，必须将其包含在实际配置中。如果您的WordPress网站尚未配置为使用Nginx运行，您可以检查WordPress文档的Nginx配置。

使用WP-Rocket的所有WordPress网站只需要一个<a title="查看与 Rocket-Nginx 相关的文章" href="https://www.wanvi.net/tag/rocket-nginx/" target="_blank" rel="noopener noreferrer">Rocket-Nginx</a>实例。也就是说，您可以根据需要生成任意数量的配置文件。

执行以下命令将模块克隆到你的Nginx安装目录：
<div class="dp-highlighter">
<div class="bar"></div>
<ol class="dp-j" start="1">
 	<li class="alt">cd /usr/local/nginx    <span class="comment">//打开安装目录</span></li>
 	<li class="">git clone https:<span class="comment">//github.com/maximejobin/rocket-nginx.git  //开始克隆库</span></li>
</ol>
</div>
从2.0版开始，必须生成配置。要生成默认配置，必须重命名禁用的ini文件并运行配置解析器：
<div class="dp-highlighter">
<div class="bar"></div>
<ol class="dp-j" start="1">
 	<li class="alt">cd rocket-nginx   <span class="comment">//打开库目录</span></li>
 	<li class="">cp rocket-nginx.ini.disabled rocket-nginx.ini    <span class="comment">//重命名文件</span></li>
 	<li class="alt">php rocket-parser.php  <span class="comment">//执行PHP生成配置文件</span></li>
</ol>
</div>
这将生成<code>default.conf</code>可包含在所有网站中的配置。如果需要更改默认配置，可以编辑ini文件并在文件底部添加另一个部分。

然后，在配置文件中，必须包含配置。如果您的网站配置已经存在<code>/etc/nginx/sites-available</code>，则需要更改配置：
<div class="dp-highlighter">
<div class="bar"></div>
<ol class="dp-j" start="1">
 	<li class="alt">server {</li>
 	<li class="">  ...</li>
 	<li class="alt"></li>
 	<li class="">  # <a title="查看与 Rocket-Nginx 相关的文章" href="https://www.wanvi.net/tag/rocket-nginx/" target="_blank" rel="noopener noreferrer">Rocket-Nginx</a> configuration</li>
 	<li class="alt">  include rocket-nginx/<span class="keyword">default</span>.conf;</li>
 	<li class=""></li>
 	<li class="alt">  ...</li>
 	<li class="">}</li>
</ol>
</div>
在重新加载配置之前，请确保对其进行测试，以免配置错误造成nginx瘫痪：nginx -t

如果没有报错我们重启nginx即可 <code>service nginx reload</code>
<h2>检验配置是否生效</h2>
您可能想要检查你的文件是否由Nginx直接提供，而不是调用任何PHP。为此，请打开<code>rocket-nginx.ini</code>文件并更改调试值：

<code>debug = false</code>

修改为：

<code>debug = true</code>

如果debug设置为0或1，则会出现以下标头：
<ul>
 	<li><strong>X-Rocket-Nginx-Serving-Static</strong>：配置是否直接服务于缓存文件（绕过WordPress）：是或否。</li>
</ul>
这会将以下标头添加到您的响应请求中：
<ul>
 	<li><strong>X-Rocket-Nginx-Reason</strong>：如果Bypass设置为“No”，那么调用WordPress的原因是什么。如果“是”，则使用的文件是什么（URL）。</li>
 	<li><strong>X-Rocket-Nginx-File</strong>：如果“是”，则使用的文件是什么（磁盘上的路径）。</li>
</ul>
<h2>无法调用缓存的几种原因</h2>
<ul>
 	<li><strong>发布请求</strong>：对Web服务器的请求是POST。这意味着数据已发送，答案可能需要与缓存文件不同（例如，发送评论时）。</li>
 	<li><strong>找到的参数</strong>：在请求中找到了一个或多个参数（例如？page = 2）。</li>
 	<li><strong>维护模式</strong>：找到.maintenance文件。因此，让我们的WordPress处理应该显示的内容。</li>
 	<li><strong>Cookie</strong>：找到一个特定的cookie并告知不提供缓存页面（例如，用户已登录，使用密码发布）。</li>
 	<li><strong>特定的移动缓存已激活</strong>：如果您在WP-Rocket中激活了特定缓存（一个用于移动缓存，一个用于桌面），HTML文件（页面，帖子等）将无法直接提供，因为Rocket-Nginx无法知道该请求是由移动或桌面设备。</li>
 	<li><strong>文件未缓存</strong>：未找到该请求的缓存文件。</li>
</ul>
<h2>总结</h2>
Wp-rocket直接将网站的求情方式从将从<strong>NGINX→PHP-FPM→PHP→静态文件</strong>变成<strong>NGINX→静态文件</strong>。换句话说，您直接从NGINX提供静态文件，而不是在提供静态文件之前将请求传递给FPM然后传递给PHP，这不仅提高了加速速度，还节省了服务器资源。

&nbsp;

&nbsp;

https://www.wanvi.net/9584.html