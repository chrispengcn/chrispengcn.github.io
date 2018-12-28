---
ID: 1361
post_title: 'ngx_http_concat_module  tengine 模块安装'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/09/13/ngx_http_concat_module-tengine-%e6%a8%a1%e5%9d%97%e5%ae%89%e8%a3%85/'
published: true
post_date: 2018-09-13 19:55:00
---
<header class="article-header">
<h1 class="article-title" data-spm-anchor-id="0.0.0.i3.289131162moXbb">ngx_http_concat_module</h1>
</header>
<div class="article-entry">

该模块类似于apache中的mod_concat模块，用于合并多个文件在一个响应报文中。

请求参数需要用两个问号（'??'）例如：
<figure class="highlight plain">
<table>
<tbody>
<tr>
<td class="code">
<pre><span class="line">http://example.com/??style1.css,style2.css,foo/style3.css</span></pre>
</td>
</tr>
</tbody>
</table>
</figure>
参数中某位置只包含一个‘?’，则'?'后表示文件的版本，例如：
<figure class="highlight plain">
<table>
<tbody>
<tr>
<td class="code">
<pre><span class="line">http://example.com/??style1.css,style2.css,foo/style3.css?v=102234</span></pre>
</td>
</tr>
</tbody>
</table>
</figure>
<figure class="highlight plain">
<table>
<tbody>
<tr>
<td class="code">
<pre><span class="line">location /static/css/ {</span>
<span class="line">    concat on;</span>
<span class="line">    concat_max_files 20;</span>
<span class="line">}</span>

<span class="line">location /static/js/ {</span>
<span class="line">    concat on;</span>
<span class="line">    concat_max_files 30;</span>
<span class="line">}</span></pre>
</td>
</tr>
</tbody>
</table>
</figure>
<h2 id="指令">指令</h2>
<blockquote><strong>concat</strong> <code>on</code> | <code>off</code>
<strong>默认:</strong> <code>concat off</code>
<strong>上下文:</strong> <code>http, server, location</code></blockquote>
在配置的地方使模块有效（失效）

<hr />

<blockquote><strong>concat_types</strong> <code>MIME types</code>
<strong>默认:</strong> <code>concat_types: text/css application/x-javascript</code>
<strong>上下文:</strong> <code>http, server, location</code></blockquote>
定义哪些<a href="http://en.wikipedia.org/wiki/MIME_type" target="_blank" rel="noopener">MIME types</a>是可以被接受

<hr />

<blockquote><strong>concat_unique</strong> <code>on</code> | <code>off</code>
<strong>默认:</strong> <code>concat_unique on</code>
<strong>上下文:</strong> <code>http, server, location</code></blockquote>
定义是否只接受在[MIME types]中的相同类型的文件，例如：
<figure class="highlight plain">
<table>
<tbody>
<tr>
<td class="code">
<pre><span class="line">http://example.com/static/??foo.css,bar/foobaz.js</span></pre>
</td>
</tr>
</tbody>
</table>
</figure>
如果配置为 'concat_unique on' 那么将返回400，如果配置为'concat_unique off'
那么将合并两个文件。

<hr />

<blockquote><strong>concat_max_files</strong> <code>number</code>
<strong>默认:</strong> <code>concat_max_files 10</code>
<strong>上下文:</strong> <code>http, server, location</code></blockquote>
定义最大能接受的文件数量。

<hr />

<blockquote><strong>concat_delimiter</strong> string
<strong>默认:</strong> 无
<strong>上下文</strong> 'http, server, location'</blockquote>
定义在文件之间添加分隔符，例如
<figure class="highlight plain">
<table>
<tbody>
<tr>
<td class="code">
<pre><span class="line">http://example.com/??1.js,2.js；</span></pre>
</td>
</tr>
</tbody>
</table>
</figure>
如果配置为<strong>concat_delimiter "\n"</strong>响应会在1.js和2.js两个文件之间插入一个换行符('\n')；

<hr />

<blockquote><strong>concat_ignore_file_error</strong> 'on | off'
<strong>默认</strong> 'concat_ignore_file_error off'
<strong>上下文</strong> 'http, server, location'</blockquote>
定义模块是否忽略文件不存在（404）或者没有权限（403）错误

<hr />

<ol>
 	<li>编译concat模块
<figure class="highlight plain">
<table>
<tbody>
<tr>
<td class="code">
<pre><span class="line" data-spm-anchor-id="0.0.0.i0.289131162moXbb">configure  [--with-http_concat_module | --with-http_concat_module=shared]</span>

<span class="line">--with-http_concat_module选项，concat模块将被静态编译到tengine中</span>

<span class="line" data-spm-anchor-id="0.0.0.i1.289131162moXbb">--with-http_concat_module=shared，concat模块将被编译成动态文件，采用动态模块的方式添加到tengine中</span></pre>
</td>
</tr>
</tbody>
</table>
</figure>
</li>
 	<li>编译,安装
<figure class="highlight plain">
<table>
<tbody>
<tr>
<td class="code">
<pre><span class="line">make &amp;&amp; make install</span></pre>
</td>
</tr>
</tbody>
</table>
</figure>
</li>
 	<li>配置concat的配置项</li>
 	<li data-spm-anchor-id="0.0.0.i2.289131162moXbb">运行</li>
</ol>
官网链接：

&nbsp;

http://tengine.taobao.org/document_cn/http_concat_cn.html

</div>