---
ID: 2510
post_title: 'Aliyun &#8211; OSS工具ossutil使用简介'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/03/04/aliyun-oss-ossutil-tool/
published: true
post_date: 2019-03-04 20:25:15
---
<h1 class="title">Aliyun - OSS工具ossutil使用简介</h1>
&nbsp;
<div class="show-content" data-note-content="">
<div class="show-content-free">
<h1>0. 简介</h1>
命令行管理工具。提供方便、简洁、丰富的Object管理命令。
官方工具，支持Windows, Linux, Mac平台，不依赖于任何第三方组件，下载后即用不需要安装。
ossutil工具旨在为用户提供一个方便的，以命令行方式管理OSS数据的途径。
当前版本未提供完整的Bucket管理功能和Multipart管理功能，相关功能会在后续版本中开发。现在如果有使用上述功能的需要，可以先使用<a href="https://help.aliyun.com/document_detail/32184.html" target="_blank" rel="nofollow noopener">osscmd</a>命令行工具。
ossutil将逐步替代osscmd，除非需要ossutil不具备的Bucket管理功能外，因此，强烈推荐使用<a href="https://help.aliyun.com/document_detail/50452.html" target="_blank" rel="nofollow noopener">ossutil</a>。
<h1>1. 下载安装</h1>
<pre class="hljs cpp"><code class="cpp">https:<span class="hljs-comment">//help.aliyun.com/document_detail/50452.html
</span></code></pre>
<pre class="pre codeblock" data-spm-anchor-id="a2c4g.11186623.2.i0.36381594bhln8f"><code class="hljs crystal">wget <span class="hljs-symbol" data-spm-anchor-id="a2c4g.11186623.2.i1.36381594bhln8f">http:</span>/<span class="hljs-regexp">/gosspublic.alicdn.com/ossutil</span><span class="hljs-regexp">/1.4.2/ossutil</span>64</code></pre>
<pre class="pre codeblock" data-spm-anchor-id="a2c4g.11186623.2.i2.36381594bhln8f"><code class="hljs angelscript" data-spm-anchor-id="a2c4g.11186623.2.i3.36381594bhln8f">chmod <span class="hljs-number">755</span> ossutil64</code></pre>
<pre class="hljs cpp"><code class="cpp"></code></pre>
<h1>2. 配置使用</h1>
<pre class="hljs ruby"><code class="ruby">交互式
$ ./ossutil64 config
请输入accessKeyID： 
请输入accessKeySecret：
请输入stsToken：
非必配项，若采用STS临时授权方式访问OSS需要配置该项，否则置空即可。stsToken生成方式参考临时访问凭证。
php, java 访问需要 stsToken</code></pre>
</div>
</div>
<h1>命令行/非交互式</h1>
<pre><code class="ruby">./ossutil64</code>config -e [endpoint二级域名] -i [<code class="ruby">accessKeyID</code>] -k [<code class="ruby">accessKeySecret</code>]</pre>
<div class="show-content" data-note-content="">
<div class="show-content-free">
<pre>/var/local/src/ossutil64 cp /[dir]/ oss://[bucketname]/[dir] -r -u
/var/local/src/ossutil64 cp /[dir]/ oss://[bucketname]/[dir] -r -u

#增量备份
#[bucketname] bucket名称
# -r 递归
# -u 更新 update</pre>
<h1></h1>
<h1>3. 帮助文档</h1>
<ul>
 	<li>执行<code>./ossutil64 --help</code></li>
 	<li>访问官网教程，<a href="https://help.aliyun.com/document_detail/50455.html" target="_blank" rel="nofollow noopener">https://help.aliyun.com/document_detail/50455.html</a></li>
 	<li>windows 客户端[护卫神]  <a href="https://www.huweishen.com/help/hwsoss/1716.html">https://www.huweishen.com/help/hwsoss/1716.html </a></li>
</ul>
</div>
</div>