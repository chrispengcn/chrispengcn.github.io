---
ID: 1402
post_title: >
  Nginx使用教程(六)：使用Nginx缓存之FastCGI缓存
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/05/nginx%e4%bd%bf%e7%94%a8%e6%95%99%e7%a8%8b%e5%85%ad%ef%bc%9a%e4%bd%bf%e7%94%a8nginx%e7%bc%93%e5%ad%98%e4%b9%8bfastcgi%e7%bc%93%e5%ad%98/'
published: true
post_date: 2018-11-05 15:36:30
---
<h1 class="postTitle"><a id="cb_post_title_url" class="postTitle2" href="https://www.cnblogs.com/felixzh/p/6283972.html">Nginx使用教程(六)：使用Nginx缓存之FastCGI缓存</a></h1>
<div class="clear"></div>
<div class="postBody">
<div id="cnblogs_post_body" class="blogpost-body">
<h2 id="启用FastCGI缓存">启用FastCGI缓存</h2>
&lt;br\&gt;
编辑必须启用缓存的虚拟主机配置文件。
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">nano /etc/nginx/sites-enabled/vhost</li>
</ol>
</div>
将以下行添加到server{}指令之外的文件顶部：
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">fastcgi_cache_path /etc/nginx/cache levels=1:2 keys_zone=MYAPP:100m inactive=60m;</li>
 	<li>fastcgi_cache_key "<span id="MathJax-Element-1-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;mi&gt;m&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;/math&gt;"><span id="MathJax-Span-1" class="math"><span id="MathJax-Span-2" class="mrow"><span id="MathJax-Span-3" class="mi">s</span><span id="MathJax-Span-4" class="mi">c</span><span id="MathJax-Span-5" class="mi">h</span><span id="MathJax-Span-6" class="mi">e</span><span id="MathJax-Span-7" class="mi">m</span><span id="MathJax-Span-8" class="mi">e</span></span></span><span class="MJX_Assistive_MathML" role="presentation">scheme</span></span>request_method<span id="MathJax-Element-2-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;o&lt;/mi&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;t&lt;/mi&gt;&lt;/math&gt;"><span id="MathJax-Span-9" class="math"><span id="MathJax-Span-10" class="mrow"><span id="MathJax-Span-11" class="mi">h</span><span id="MathJax-Span-12" class="mi">o</span><span id="MathJax-Span-13" class="mi">s</span><span id="MathJax-Span-14" class="mi">t</span></span></span><span class="MJX_Assistive_MathML" role="presentation">host</span></span>request_uri";</li>
</ol>
</div>
“fastcgi_cache_path”指令指定缓存（/etc/<a href="https://www.centos.bz/category/web-server/nginx/">nginx</a>/cache）的位置，其大小（100m），内存区域名称（MYAPP），子目录级别和非活动定时器。
位置可以在硬盘上的任何地方; 但是，大小必须小于您的服务器的RAM +交换，否则你会收到一个错误，“无法分配内存”。 如果缓存在“inactive”选项指定的特定时间内没有被访问（这里为60分钟），<a href="https://www.centos.bz/tag/nginx-2/">Nginx</a>将删除它。
“fastcgi_cache_key”指令指定如何哈希缓存文件名。 Nginx基于此指令使用MD5加密访问的文件。
在location ~ .php$ { }里添加如下行：
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">fastcgi_cache MYAPP;</li>
 	<li>fastcgi_cache_valid 200 60m;</li>
</ol>
</div>
“fastcgi_cache”指令引用我们在“fastcgicache_path”指令中指定的内存区域名称，并将缓存存储在此区域中。
默认情况下，Nginx根据这些响应头里指定的时间决定存储缓存对象的时间：X-Accel-Expires / Expires / Cache-Control。
如果缺少这些头，“fastcgi_cache_valid”指令用于指定默认缓存生命周期。 在上面输入的语句中，只缓存状态代码为200的响应。 也可以指定其他响应代码。
测试配置：
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">service nginx configtest</li>
</ol>
</div>
重载Nginx:
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">service nginx reload</li>
</ol>
</div>
完整的vhost配置文件如下：
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">fastcgi_cache_path /etc/nginx/cache levels=1:2 keys_zone=MYAPP:100m inactive=60m;</li>
 	<li>fastcgi_cache_key "<span id="MathJax-Element-3-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;mi&gt;m&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;/math&gt;"><span id="MathJax-Span-15" class="math"><span id="MathJax-Span-16" class="mrow"><span id="MathJax-Span-17" class="mi">s</span><span id="MathJax-Span-18" class="mi">c</span><span id="MathJax-Span-19" class="mi">h</span><span id="MathJax-Span-20" class="mi">e</span><span id="MathJax-Span-21" class="mi">m</span><span id="MathJax-Span-22" class="mi">e</span></span></span><span class="MJX_Assistive_MathML" role="presentation">scheme</span></span>request_method<span id="MathJax-Element-4-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;o&lt;/mi&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;t&lt;/mi&gt;&lt;/math&gt;"><span id="MathJax-Span-23" class="math"><span id="MathJax-Span-24" class="mrow"><span id="MathJax-Span-25" class="mi">h</span><span id="MathJax-Span-26" class="mi">o</span><span id="MathJax-Span-27" class="mi">s</span><span id="MathJax-Span-28" class="mi">t</span></span></span><span class="MJX_Assistive_MathML" role="presentation">host</span></span>request_uri";</li>
 	<li></li>
 	<li>server {</li>
 	<li>    listen   80;</li>
 	<li></li>
 	<li>    root /usr/share/nginx/html;</li>
 	<li>    index index.php index.html index.htm;</li>
 	<li></li>
 	<li>    server_name example.com;</li>
 	<li></li>
 	<li>    location / {</li>
 	<li>        try_files <span id="MathJax-Element-5-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;u&lt;/mi&gt;&lt;mi&gt;r&lt;/mi&gt;&lt;mi&gt;i&lt;/mi&gt;&lt;/math&gt;"><span id="MathJax-Span-29" class="math"><span id="MathJax-Span-30" class="mrow"><span id="MathJax-Span-31" class="mi">u</span><span id="MathJax-Span-32" class="mi">r</span><span id="MathJax-Span-33" class="mi">i</span></span></span><span class="MJX_Assistive_MathML" role="presentation">uri</span></span>uri/ /index.html;</li>
 	<li>    }</li>
 	<li></li>
 	<li>    location ~ \.php$ {</li>
 	<li>        try_files $uri =404;</li>
 	<li>        fastcgi_pass unix:/var/run/php5-fpm.sock;</li>
 	<li>        fastcgi_index index.php;</li>
 	<li>        include fastcgi_params;</li>
 	<li>        fastcgi_cache MYAPP;</li>
 	<li>        fastcgi_cache_valid 200 60m;</li>
 	<li>    }</li>
 	<li>}</li>
</ol>
</div>
<h2 id="测试FastCGI缓存是否生效">测试FastCGI缓存是否生效</h2>
&lt;br\&gt;
创建/usr/share/nginx/html/time.php，内容如下：
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">&lt;?php</li>
 	<li>echo time();</li>
 	<li>?&gt;</li>
</ol>
</div>
使用curl或您的Web浏览器多次请求此文件。
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">root@droplet:~# curl http://localhost/time.php;echo</li>
 	<li>1382986152</li>
 	<li>root@droplet:~# curl http://localhost/time.php;echo</li>
 	<li>1382986152</li>
 	<li>root@droplet:~# curl http://localhost/time.php;echo</li>
 	<li>1382986152</li>
</ol>
</div>
如果缓存工作正常，您应该在缓存响应时在所有请求上看到相同的时间戳。
执行缓存位置的递归列表以查找此请求的缓存。
root@droplet:~# ls -lR /etc/nginx/cache/
/etc/nginx/cache/:
total 0
drwx—— 3 www-data www-data 60 Oct 28 18:53 e

/etc/nginx/cache/e:
total 0
drwx—— 2 www-data www-data 60 Oct 28 18:53 18

/etc/nginx/cache/e/18:
total 4
-rw——- 1 www-data www-data 117 Oct 28 18:53 b777c8adab3ec92cd43756226caf618e
我们还可以使Nginx为响应添加一个“X-Cache”头，指示缓存是否被丢失或命中。
在server{}指令上面添加以下内容：
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">add_header X-Cache $upstream_cache_status;</li>
</ol>
</div>
重新加载Nginx服务，并使用curl执行详细请求以查看新标题。
root@droplet:~# curl -v http://localhost/time.php
* About to connect() to localhost port 80 (#0)
* Trying 127.0.0.1…
* connected
* Connected to localhost (127.0.0.1) port 80 (#0)
&gt; GET /time.php HTTP/1.1
&gt; User-Agent: curl/7.26.0
&gt; Host: localhost
&gt; Accept: */*
&gt;
* HTTP 1.1 or later with persistent connection, pipelining supported
&lt; HTTP/1.1 200 OK &lt; Server: nginx &lt; Date: Tue, 29 Oct 2013 11:24:04 GMT &lt; Content-Type: text/html &lt; Transfer-Encoding: chunked &lt; Connection: keep-alive &lt; X-Cache: HIT &lt; * Connection #0 to host localhost left intact 1383045828* Closing connection #0
<h2 id="不需要缓存的页面">不需要缓存的页面</h2>
&lt;br\&gt;
某些动态内容（例如认证所需页面）不应缓存。 可以基于诸如“requesturi”，“requestmethod”和“http_cookie”的服务器变量来排除这样的内容被高速缓存。
如下例子:
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">#Cache everything by default</li>
 	<li>set $no_cache 0;</li>
 	<li></li>
 	<li>#Don't cache POST requests</li>
 	<li>if ($request_method = POST)</li>
 	<li>{</li>
 	<li>    set $no_cache 1;</li>
 	<li>}</li>
 	<li></li>
 	<li>#Don't cache if the URL contains a query string</li>
 	<li>if ($query_string != "")</li>
 	<li>{</li>
 	<li>    set $no_cache 1;</li>
 	<li>}</li>
 	<li></li>
 	<li>#Don't cache the following URLs</li>
 	<li>if ($request_uri ~* "/(administrator/|login.php)")</li>
 	<li>{</li>
 	<li>    set $no_cache 1;</li>
 	<li>}</li>
 	<li></li>
 	<li>#Don't cache if there is a cookie called PHPSESSID</li>
 	<li>if ($http_cookie = "PHPSESSID")</li>
 	<li>{</li>
 	<li>    set $no_cache 1;</li>
 	<li>}</li>
</ol>
</div>
要将“<span id="MathJax-Element-6-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;n&lt;/mi&gt;&lt;msub&gt;&lt;mi&gt;o&lt;/mi&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;/msub&gt;&lt;mi&gt;a&lt;/mi&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;mo&gt;&amp;#x201D;&lt;/mo&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x53D8;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x91CF;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x5E94;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x7528;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x5230;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x76F8;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x5E94;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x7684;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x6307;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x4EE4;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#xFF0C;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x8BF7;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x5C06;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x4EE5;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x4E0B;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x884C;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x653E;&lt;/mo&gt;&lt;/mrow&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x5728;&lt;/mo&gt;&lt;/mrow&gt;&lt;mi&gt;l&lt;/mi&gt;&lt;mi&gt;o&lt;/mi&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;mi&gt;a&lt;/mi&gt;&lt;mi&gt;t&lt;/mi&gt;&lt;mi&gt;i&lt;/mi&gt;&lt;mi&gt;o&lt;/mi&gt;&lt;mi&gt;n&lt;/mi&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;&amp;#x301C;&lt;/mo&gt;&lt;/mrow&gt;&lt;mo&gt;.&lt;/mo&gt;&lt;mi&gt;p&lt;/mi&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;p&lt;/mi&gt;&lt;/math&gt;"><span id="MathJax-Span-34" class="math"><span id="MathJax-Span-35" class="mrow"><span id="MathJax-Span-36" class="mi">n</span><span id="MathJax-Span-37" class="msubsup"><span id="MathJax-Span-38" class="mi">o</span><span id="MathJax-Span-39" class="mi">c</span></span><span id="MathJax-Span-40" class="mi">a</span><span id="MathJax-Span-41" class="mi">c</span><span id="MathJax-Span-42" class="mi">h</span><span id="MathJax-Span-43" class="mi">e</span><span id="MathJax-Span-44" class="mo">”</span><span id="MathJax-Span-45" class="texatom"><span id="MathJax-Span-46" class="mrow"><span id="MathJax-Span-47" class="mo">变</span></span></span><span id="MathJax-Span-48" class="texatom"><span id="MathJax-Span-49" class="mrow"><span id="MathJax-Span-50" class="mo">量</span></span></span><span id="MathJax-Span-51" class="texatom"><span id="MathJax-Span-52" class="mrow"><span id="MathJax-Span-53" class="mo">应</span></span></span><span id="MathJax-Span-54" class="texatom"><span id="MathJax-Span-55" class="mrow"><span id="MathJax-Span-56" class="mo">用</span></span></span><span id="MathJax-Span-57" class="texatom"><span id="MathJax-Span-58" class="mrow"><span id="MathJax-Span-59" class="mo">到</span></span></span><span id="MathJax-Span-60" class="texatom"><span id="MathJax-Span-61" class="mrow"><span id="MathJax-Span-62" class="mo">相</span></span></span><span id="MathJax-Span-63" class="texatom"><span id="MathJax-Span-64" class="mrow"><span id="MathJax-Span-65" class="mo">应</span></span></span><span id="MathJax-Span-66" class="texatom"><span id="MathJax-Span-67" class="mrow"><span id="MathJax-Span-68" class="mo">的</span></span></span><span id="MathJax-Span-69" class="texatom"><span id="MathJax-Span-70" class="mrow"><span id="MathJax-Span-71" class="mo">指</span></span></span><span id="MathJax-Span-72" class="texatom"><span id="MathJax-Span-73" class="mrow"><span id="MathJax-Span-74" class="mo">令</span></span></span><span id="MathJax-Span-75" class="texatom"><span id="MathJax-Span-76" class="mrow"><span id="MathJax-Span-77" class="mo">，</span></span></span><span id="MathJax-Span-78" class="texatom"><span id="MathJax-Span-79" class="mrow"><span id="MathJax-Span-80" class="mo">请</span></span></span><span id="MathJax-Span-81" class="texatom"><span id="MathJax-Span-82" class="mrow"><span id="MathJax-Span-83" class="mo">将</span></span></span><span id="MathJax-Span-84" class="texatom"><span id="MathJax-Span-85" class="mrow"><span id="MathJax-Span-86" class="mo">以</span></span></span><span id="MathJax-Span-87" class="texatom"><span id="MathJax-Span-88" class="mrow"><span id="MathJax-Span-89" class="mo">下</span></span></span><span id="MathJax-Span-90" class="texatom"><span id="MathJax-Span-91" class="mrow"><span id="MathJax-Span-92" class="mo">行</span></span></span><span id="MathJax-Span-93" class="texatom"><span id="MathJax-Span-94" class="mrow"><span id="MathJax-Span-95" class="mo">放</span></span></span><span id="MathJax-Span-96" class="texatom"><span id="MathJax-Span-97" class="mrow"><span id="MathJax-Span-98" class="mo">在</span></span></span><span id="MathJax-Span-99" class="mi">l</span><span id="MathJax-Span-100" class="mi">o</span><span id="MathJax-Span-101" class="mi">c</span><span id="MathJax-Span-102" class="mi">a</span><span id="MathJax-Span-103" class="mi">t</span><span id="MathJax-Span-104" class="mi">i</span><span id="MathJax-Span-105" class="mi">o</span><span id="MathJax-Span-106" class="mi">n</span><span id="MathJax-Span-107" class="texatom"><span id="MathJax-Span-108" class="mrow"><span id="MathJax-Span-109" class="mo">〜</span></span></span><span id="MathJax-Span-110" class="mo">.</span><span id="MathJax-Span-111" class="mi">p</span><span id="MathJax-Span-112" class="mi">h</span><span id="MathJax-Span-113" class="mi">p</span></span></span><span class="MJX_Assistive_MathML" role="presentation">nocache”变量应用到相应的指令，请将以下行放在location〜.php</span></span> {}中，
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">fastcgi_cache_bypass $no_cache;</li>
 	<li>fastcgi_no_cache $no_cache;</li>
</ol>
</div>
“fasctcgicachebypass”指令忽略之前由我们设置的条件相关的请求的现有缓存。 如果满足指定的条件，“fastcginocache”指令不缓存请求。
<h2 id="清除缓存">清除缓存</h2>
&lt;br\&gt;
缓存的命名约定基于我们为“fastcgicachekey”指令设置的变量。
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">fastcgi_cache_key "<span id="MathJax-Element-7-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;mi&gt;m&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;/math&gt;"><span id="MathJax-Span-114" class="math"><span id="MathJax-Span-115" class="mrow"><span id="MathJax-Span-116" class="mi">s</span><span id="MathJax-Span-117" class="mi">c</span><span id="MathJax-Span-118" class="mi">h</span><span id="MathJax-Span-119" class="mi">e</span><span id="MathJax-Span-120" class="mi">m</span><span id="MathJax-Span-121" class="mi">e</span></span></span><span class="MJX_Assistive_MathML" role="presentation">scheme</span></span>request_method<span id="MathJax-Element-8-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;o&lt;/mi&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;t&lt;/mi&gt;&lt;/math&gt;"><span id="MathJax-Span-122" class="math"><span id="MathJax-Span-123" class="mrow"><span id="MathJax-Span-124" class="mi">h</span><span id="MathJax-Span-125" class="mi">o</span><span id="MathJax-Span-126" class="mi">s</span><span id="MathJax-Span-127" class="mi">t</span></span></span><span class="MJX_Assistive_MathML" role="presentation">host</span></span>request_uri";</li>
</ol>
</div>
根据这些变量，当我们请求“http//localhost/time.php”时，以下是实际值：
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">fastcgi_cache_key "httpGETlocalhost/time.php";</li>
</ol>
</div>
将此字符串传递到MD5哈希将输出以下字符串：
b777c8adab3ec92cd43756226caf618e
这就是高速缓存的文件名，就像我们输入的“levels = 1：2”子目录。 因此，目录的第一级将从这个MD5字符串的最后一个字符命名为1个字符，即e; 第二级将具有在第一级之后的最后2个字符，即18.因此，该高速缓存的整个目录结构如下：
/etc/nginx/cache/e/18/b777c8adab3ec92cd43756226caf618e
基于这种缓存命名格式，您可以用您最喜欢的语言开发一个清除脚本。 对于本教程，我将提供一个简单的PHP脚本，它清除POSTed URL的缓存。
/usr/share/nginx/html/purge.php：
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">&lt;?php</li>
 	<li>$cache_path = '/etc/nginx/cache/';</li>
 	<li><span id="MathJax-Element-9-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;u&lt;/mi&gt;&lt;mi&gt;r&lt;/mi&gt;&lt;mi&gt;l&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;mi&gt;p&lt;/mi&gt;&lt;mi&gt;a&lt;/mi&gt;&lt;mi&gt;r&lt;/mi&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;msub&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;mi&gt;u&lt;/mi&gt;&lt;/msub&gt;&lt;mi&gt;r&lt;/mi&gt;&lt;mi&gt;l&lt;/mi&gt;&lt;mo stretchy=&quot;false&quot;&gt;(&lt;/mo&gt;&lt;/math&gt;"><span id="MathJax-Span-128" class="math"><span id="MathJax-Span-129" class="mrow"><span id="MathJax-Span-130" class="mi">u</span><span id="MathJax-Span-131" class="mi">r</span><span id="MathJax-Span-132" class="mi">l</span><span id="MathJax-Span-133" class="mo">=</span><span id="MathJax-Span-134" class="mi">p</span><span id="MathJax-Span-135" class="mi">a</span><span id="MathJax-Span-136" class="mi">r</span><span id="MathJax-Span-137" class="mi">s</span><span id="MathJax-Span-138" class="msubsup"><span id="MathJax-Span-139" class="mi">e</span><span id="MathJax-Span-140" class="mi">u</span></span><span id="MathJax-Span-141" class="mi">r</span><span id="MathJax-Span-142" class="mi">l</span><span id="MathJax-Span-143" class="mo">(</span></span></span><span class="MJX_Assistive_MathML" role="presentation">url=parseurl(</span></span>_POST['url']);</li>
 	<li>if(!$url)</li>
 	<li>{</li>
 	<li>    echo 'Invalid URL entered';</li>
 	<li>    die();</li>
 	<li>}</li>
 	<li><span id="MathJax-Element-10-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;mi&gt;m&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;/math&gt;"><span id="MathJax-Span-144" class="math"><span id="MathJax-Span-145" class="mrow"><span id="MathJax-Span-146" class="mi">s</span><span id="MathJax-Span-147" class="mi">c</span><span id="MathJax-Span-148" class="mi">h</span><span id="MathJax-Span-149" class="mi">e</span><span id="MathJax-Span-150" class="mi">m</span><span id="MathJax-Span-151" class="mi">e</span><span id="MathJax-Span-152" class="mo">=</span></span></span><span class="MJX_Assistive_MathML" role="presentation">scheme=</span></span>url['scheme'];</li>
 	<li><span id="MathJax-Element-11-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;o&lt;/mi&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;t&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;/math&gt;"><span id="MathJax-Span-153" class="math"><span id="MathJax-Span-154" class="mrow"><span id="MathJax-Span-155" class="mi">h</span><span id="MathJax-Span-156" class="mi">o</span><span id="MathJax-Span-157" class="mi">s</span><span id="MathJax-Span-158" class="mi">t</span><span id="MathJax-Span-159" class="mo">=</span></span></span><span class="MJX_Assistive_MathML" role="presentation">host=</span></span>url['host'];</li>
 	<li><span id="MathJax-Element-12-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;r&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;mi&gt;q&lt;/mi&gt;&lt;mi&gt;u&lt;/mi&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;t&lt;/mi&gt;&lt;mi&gt;u&lt;/mi&gt;&lt;mi&gt;r&lt;/mi&gt;&lt;mi&gt;i&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;/math&gt;"><span id="MathJax-Span-160" class="math"><span id="MathJax-Span-161" class="mrow"><span id="MathJax-Span-162" class="mi">r</span><span id="MathJax-Span-163" class="mi">e</span><span id="MathJax-Span-164" class="mi">q</span><span id="MathJax-Span-165" class="mi">u</span><span id="MathJax-Span-166" class="mi">e</span><span id="MathJax-Span-167" class="mi">s</span><span id="MathJax-Span-168" class="mi">t</span><span id="MathJax-Span-169" class="mi">u</span><span id="MathJax-Span-170" class="mi">r</span><span id="MathJax-Span-171" class="mi">i</span><span id="MathJax-Span-172" class="mo">=</span></span></span><span class="MJX_Assistive_MathML" role="presentation">requesturi=</span></span>url['path'];</li>
 	<li><span id="MathJax-Element-13-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;a&lt;/mi&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mo&gt;=&lt;/mo&gt;&lt;mi&gt;m&lt;/mi&gt;&lt;mi&gt;d&lt;/mi&gt;&lt;mn&gt;5&lt;/mn&gt;&lt;mo stretchy=&quot;false&quot;&gt;(&lt;/mo&gt;&lt;/math&gt;"><span id="MathJax-Span-173" class="math"><span id="MathJax-Span-174" class="mrow"><span id="MathJax-Span-175" class="mi">h</span><span id="MathJax-Span-176" class="mi">a</span><span id="MathJax-Span-177" class="mi">s</span><span id="MathJax-Span-178" class="mi">h</span><span id="MathJax-Span-179" class="mo">=</span><span id="MathJax-Span-180" class="mi">m</span><span id="MathJax-Span-181" class="mi">d</span><span id="MathJax-Span-182" class="mn">5</span><span id="MathJax-Span-183" class="mo">(</span></span></span><span class="MJX_Assistive_MathML" role="presentation">hash=md5(</span></span>scheme.'GET'.<span id="MathJax-Element-14-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;o&lt;/mi&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;t&lt;/mi&gt;&lt;mo&gt;.&lt;/mo&gt;&lt;/math&gt;"><span id="MathJax-Span-184" class="math"><span id="MathJax-Span-185" class="mrow"><span id="MathJax-Span-186" class="mi">h</span><span id="MathJax-Span-187" class="mi">o</span><span id="MathJax-Span-188" class="mi">s</span><span id="MathJax-Span-189" class="mi">t</span><span id="MathJax-Span-190" class="mo">.</span></span></span><span class="MJX_Assistive_MathML" role="presentation">host.</span></span>requesturi);</li>
 	<li>var_dump(unlink(<span id="MathJax-Element-15-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;mi&gt;a&lt;/mi&gt;&lt;mi&gt;c&lt;/mi&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;msub&gt;&lt;mi&gt;e&lt;/mi&gt;&lt;mi&gt;p&lt;/mi&gt;&lt;/msub&gt;&lt;mi&gt;a&lt;/mi&gt;&lt;mi&gt;t&lt;/mi&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mo&gt;.&lt;/mo&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;u&lt;/mi&gt;&lt;mi&gt;b&lt;/mi&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;t&lt;/mi&gt;&lt;mi&gt;r&lt;/mi&gt;&lt;mo stretchy=&quot;false&quot;&gt;(&lt;/mo&gt;&lt;/math&gt;"><span id="MathJax-Span-191" class="math"><span id="MathJax-Span-192" class="mrow"><span id="MathJax-Span-193" class="mi">c</span><span id="MathJax-Span-194" class="mi">a</span><span id="MathJax-Span-195" class="mi">c</span><span id="MathJax-Span-196" class="mi">h</span><span id="MathJax-Span-197" class="msubsup"><span id="MathJax-Span-198" class="mi">e</span><span id="MathJax-Span-199" class="mi">p</span></span><span id="MathJax-Span-200" class="mi">a</span><span id="MathJax-Span-201" class="mi">t</span><span id="MathJax-Span-202" class="mi">h</span><span id="MathJax-Span-203" class="mo">.</span><span id="MathJax-Span-204" class="mi">s</span><span id="MathJax-Span-205" class="mi">u</span><span id="MathJax-Span-206" class="mi">b</span><span id="MathJax-Span-207" class="mi">s</span><span id="MathJax-Span-208" class="mi">t</span><span id="MathJax-Span-209" class="mi">r</span><span id="MathJax-Span-210" class="mo">(</span></span></span><span class="MJX_Assistive_MathML" role="presentation">cachepath.substr(</span></span>hash, -1) . '/' . substr(<span id="MathJax-Element-16-Frame" class="MathJax" style="margin: 0px; padding: 0px; display: inline; font-style: normal; font-weight: normal; line-height: 1.5; font-size: 13px; text-indent: 0px; text-align: left; text-transform: none; letter-spacing: normal; word-spacing: normal; overflow-wrap: normal; white-space: nowrap; float: none; direction: ltr; max-width: none; max-height: none; min-width: 0px; min-height: 0px; border: 0px; position: relative;" tabindex="0" role="presentation" data-mathml="&lt;math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mi&gt;a&lt;/mi&gt;&lt;mi&gt;s&lt;/mi&gt;&lt;mi&gt;h&lt;/mi&gt;&lt;mo&gt;,&lt;/mo&gt;&lt;mo&gt;&amp;#x2212;&lt;/mo&gt;&lt;mn&gt;3&lt;/mn&gt;&lt;mo&gt;,&lt;/mo&gt;&lt;mn&gt;2&lt;/mn&gt;&lt;mo stretchy=&quot;false&quot;&gt;)&lt;/mo&gt;&lt;msup&gt;&lt;mo&gt;.&lt;/mo&gt;&lt;mo&gt;&amp;#x2032;&lt;/mo&gt;&lt;/msup&gt;&lt;msup&gt;&lt;mrow class=&quot;MJX-TeXAtom-ORD&quot;&gt;&lt;mo&gt;/&lt;/mo&gt;&lt;/mrow&gt;&lt;mo&gt;&amp;#x2032;&lt;/mo&gt;&lt;/msup&gt;&lt;mo&gt;.&lt;/mo&gt;&lt;/math&gt;"><span id="MathJax-Span-211" class="math"><span id="MathJax-Span-212" class="mrow"><span id="MathJax-Span-213" class="mi">h</span><span id="MathJax-Span-214" class="mi">a</span><span id="MathJax-Span-215" class="mi">s</span><span id="MathJax-Span-216" class="mi">h</span><span id="MathJax-Span-217" class="mo">,</span><span id="MathJax-Span-218" class="mo">−</span><span id="MathJax-Span-219" class="mn">3</span><span id="MathJax-Span-220" class="mo">,</span><span id="MathJax-Span-221" class="mn">2</span><span id="MathJax-Span-222" class="mo">)</span><span id="MathJax-Span-223" class="msup"><span id="MathJax-Span-224" class="mo">.</span><span id="MathJax-Span-225" class="mo">′</span></span><span id="MathJax-Span-226" class="msup"><span id="MathJax-Span-227" class="texatom"><span id="MathJax-Span-228" class="mrow"><span id="MathJax-Span-229" class="mo">/</span></span></span><span id="MathJax-Span-230" class="mo">′</span></span><span id="MathJax-Span-231" class="mo">.</span></span></span><span class="MJX_Assistive_MathML" role="presentation">hash,−3,2).′/′.</span></span>hash));</li>
 	<li>?&gt;</li>
</ol>
</div>
向此文件发送带需要清除的URL的POST请求。
<div class="hl-surround">
<ol class="hl-main ln-show" title="Double click to hide line number.">
 	<li class="hl-firstline">curl -d 'url=http://www.example.com/time.php' http://localhost/purge.php</li>
</ol>
</div>
该脚本将根据是否清除缓存而输出true或false。 请确保从高速缓存中排除此脚本，并限制访问。

</div>
<div id="MySignature">软件-注重思想、逻辑</div>
</div>