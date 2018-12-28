---
ID: 1249
post_title: >
  Vultr
  Centos安装shadowsocks服务端并开启BBR加速
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/30/vultr-centos%e5%ae%89%e8%a3%85shadowsocks%e6%9c%8d%e5%8a%a1%e7%ab%af%e5%b9%b6%e5%bc%80%e5%90%afbbr%e5%8a%a0%e9%80%9f/'
published: true
post_date: 2018-05-30 22:51:16
---
<h4>阿里云优惠券：</h4>
http://aliyun.hss5.com
<h4></h4>
<h4>开启TCP Fast Open</h4>
<blockquote>
<pre class="hljs vala"><code>vim /etc/rc.local

<span class="hljs-meta"># 在最后一行增加以下内容</span>
echo <span class="hljs-number">3</span> &gt; /proc/sys/net/ipv4/tcp_fastopen

<span class="hljs-meta"># 然后</span>
vim /etc/sysctl.conf

<span class="hljs-meta"># 在最后一行增加：</span>
net.ipv4.tcp_fastopen = <span class="hljs-number">3</span>

<span class="hljs-meta"># 编辑配置文件</span>
vi /etc/shadowsocks.json
<span class="hljs-meta"># 添加一项</span>
<span class="hljs-string">"fast_open"</span>:<span class="hljs-literal">true</span>

<span class="hljs-meta"># 最后重启</span>
ssserver -c /etc/shadowsocks.json -d restart 


</code></pre>
</blockquote>
<h4>软件加速</h4>
加速有锐速加速和Google BBR加速。我这里使用的是BBR加速

我这里参考的：<a href="https://teddysun.com/489.html" target="_blank" rel="nofollow noopener noreferrer">https://teddysun.com/489.html</a>

<a href="https://segmentfault.com/a/1190000011511512">https://segmentfault.com/a/1190000011511512</a>

安装一键脚本
<blockquote>
<pre class="hljs vim"><code>wget --<span class="hljs-keyword">no</span>-check-certificate http<span class="hljs-variable">s:</span>//github.<span class="hljs-keyword">com</span>/teddysun/across/raw/master/bbr.<span class="hljs-keyword">sh</span>
chmod +<span class="hljs-keyword">x</span> bbr.<span class="hljs-keyword">sh</span>
./bbr.<span class="hljs-keyword">sh</span></code></pre>
</blockquote>
安装按成后会提示重启，重启完成后：
查看内核：
<pre class="hljs ebnf"><code><span class="hljs-attribute">uname -r</span></code></pre>
包含4.13就说明内核替换成功

检查是否开启BBR
<div class="widget-codetool"></div>
<blockquote>
<pre class="hljs vala"><code>sysctl net.ipv4.tcp_available_congestion_control
<span class="hljs-meta"># 返回值一般为：net.ipv4.tcp_available_congestion_control = bbr cubic reno</span>

sysctl net.ipv4.tcp_congestion_control
<span class="hljs-meta"># 返回值一般为：net.ipv4.tcp_congestion_control = bbr</span>

sysctl net.core.default_qdisc
<span class="hljs-meta"># 返回值一般为：net.core.default_qdisc = fq</span>

lsmod | grep bbr
<span class="hljs-meta"># 返回值有tcp_bbr则说明已经启动</span></code></pre>
</blockquote>