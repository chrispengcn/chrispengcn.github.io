---
ID: 1430
post_title: 'postfix报错postfix: fatal: parameter inet_interfaces: no local interface found for ::1'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/15/postfix%e6%8a%a5%e9%94%99postfix-fatal-parameter-inet_interfaces-no-local-interface-found-for-1/'
published: true
post_date: 2018-11-15 11:23:56
---
启动postfix出错：

<code>postfix: fatal: parameter inet_interfaces: no local interface found for ::1
</code>
解决：
<code>vi /etc/postfix/main.cf</code>
发现配置为：
<pre class="prettyprint"><code class="hljs ini has-numbering"><span class="hljs-setting">inet_interfaces = <span class="hljs-value">localhost</span></span>
<span class="hljs-setting">inet_protocols = <span class="hljs-value">all</span></span></code></pre>
<ul class="pre-numbering">
 	<li>1</li>
 	<li>2</li>
</ul>
改成：
<pre class="prettyprint"><code class="hljs glsl has-numbering">inet_interfaces = <span class="hljs-built_in">all</span>
inet_protocols = <span class="hljs-built_in">all</span></code></pre>
<ul class="pre-numbering">
 	<li>1</li>
 	<li>2</li>
</ul>
重新启动
service postfix start
OK！