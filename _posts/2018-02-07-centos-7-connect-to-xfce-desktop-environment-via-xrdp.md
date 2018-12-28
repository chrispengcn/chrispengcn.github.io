---
ID: 1116
post_title: 'CentOS 7: Connect to Xfce desktop environment via XRDP'
author: chrispengcn
post_excerpt: |
  CentOS 7: Connect to Xfce desktop environment via XRDP
  
  This article will describe installing Xfce desktop environment and XRDP, and connecting to Xfce desktop environment via XRDP.
layout: post
permalink: >
  http://hss5.com/2018/02/07/centos-7-connect-to-xfce-desktop-environment-via-xrdp/
published: true
post_date: 2018-02-07 09:48:22
---
<div class="n j-blog-meta j-blog-post--header">
<h1 class="j-blog-header j-blog-headline j-blog-post--headline">CentOS 7: Connect to Xfce desktop environment via XRDP</h1>
</div>
<div class="post j-blog-content">
<div id="cc-matrix-3605866192">
<div id="cc-m-12929683492" class="j-module n j-text ">

This article will describe installing Xfce desktop environment and XRDP, and connecting to Xfce desktop environment via XRDP.

</div>
<div id="cc-m-12929683592" class="j-module n j-spacing "></div>
<div id="cc-m-12929683692" class="j-module n j-sharebuttons "></div>
<div id="cc-m-12929683792" class="j-module n j-text ">
<div id="content">
<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
 	<li><a href="https://www.hiroom2.com/2017/10/01/centos-7-xrdp-xfce-en/#sec-1">1. Install Xfce desktop environment</a></li>
 	<li><a href="https://www.hiroom2.com/2017/10/01/centos-7-xrdp-xfce-en/#sec-2">2. Install XRDP</a></li>
 	<li><a href="https://www.hiroom2.com/2017/10/01/centos-7-xrdp-xfce-en/#sec-3">3. Create ~/.Xclients</a></li>
 	<li><a href="https://www.hiroom2.com/2017/10/01/centos-7-xrdp-xfce-en/#sec-4">4. Connect to Xfce desktop environment via XRDP</a></li>
</ul>
</div>
</div>
<div id="outline-container-sec-1" class="outline-2">
<h2 id="sec-1"><span class="section-number-2">1</span> Install Xfce desktop environment</h2>
<div id="text-1" class="outline-text-2">

Install Xfce desktop environment with <a href="https://www.hiroom2.com/2017/07/26/centos-7-xfce-en/">this</a>.

</div>
</div>
<div id="outline-container-sec-2" class="outline-2">
<h2 id="sec-2"><span class="section-number-2">2</span> Install XRDP</h2>
<div id="text-2" class="outline-text-2">

Install XRDP.
<blockquote>
<pre class="example">$ sudo yum install -y epel-release
$ sudo yum install -y xrdp
$ sudo systemctl enable xrdp
$ sudo systemctl start xrdp
</pre>
</blockquote>
Open XRDP port.
<blockquote>
<pre class="example">$ sudo firewall-cmd --add-port=3389/tcp --permanent
$ sudo firewall-cmd --reload
</pre>
</blockquote>
</div>
</div>
<div id="outline-container-sec-3" class="outline-2">
<h2 id="sec-3"><span class="section-number-2">3</span> Create ~/.Xclients</h2>
<div id="text-3" class="outline-text-2">

Create .Xclients in home directory of user to be connected.
<blockquote>
<pre class="example">$ echo "xfce4-session" &gt; ~/.Xclients
$ chmod a+x ~/.Xclients
</pre>
</blockquote>
</div>
</div>
<div id="outline-container-sec-4" class="outline-2">
<h2 id="sec-4"><span class="section-number-2">4</span> Connect to Xfce desktop environment via XRDP</h2>
<div id="text-4" class="outline-text-2">

The rdesktop connection is as the following.
<div class="figure">

<img src="https://dl-web.dropbox.com/s/4gxel5d89mx5uiy/0001_xrdp-xfce.png" alt="0001_xrdp-xfce.png" width="640px" vspace="15px" />

</div>
install xfce4

</div>
<code>yum groupinstall xfce4</code>

or  <code>yum</code> install xfce*

</div>
</div>
</div>
</div>
</div>