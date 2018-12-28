---
ID: 1526
post_title: 'CentOS 7: Install Xfce for desktop environment'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/11/29/centos-7-install-xfce-for-desktop-environment/
published: true
post_date: 2018-11-29 09:09:56
---
<div class="n j-blog-meta j-blog-post--header">
<h1 class="j-blog-header j-blog-headline j-blog-post--headline">CentOS 7: Install Xfce for desktop environment</h1>
</div>
<div class="post j-blog-content">
<div id="cc-matrix-3591349492">
<div id="cc-m-12871265892" class="j-module n j-text ">

This article will describe installing Xfce for desktop environment.

</div>
<div id="cc-m-12871265992" class="j-module n j-spacing ">
<div class="cc-m-spacer"></div>
</div>
<div id="cc-m-12871266092" class="j-module n j-sharebuttons ">
<div class="cc-sharebuttons-element cc-sharebuttons-size-32 cc-sharebuttons-style-colored cc-sharebuttons-design-square cc-sharebuttons-align-left"></div>
</div>
<div id="cc-m-12871266192" class="j-module n j-text ">
<div id="content">
<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
 	<li><a href="https://www.hiroom2.com/2017/07/26/centos-7-xfce-en/#sec-1">1. Install Xfce</a></li>
 	<li><a href="https://www.hiroom2.com/2017/07/26/centos-7-xfce-en/#sec-2">2. Login to Xfce</a></li>
 	<li><a href="https://www.hiroom2.com/2017/07/26/centos-7-xfce-en/#sec-3">3. Uninstall Xfce</a></li>
</ul>
</div>
</div>
<div id="outline-container-sec-1" class="outline-2">
<h2 id="sec-1"><span class="section-number-2">1</span> Install Xfce</h2>
<div id="text-1" class="outline-text-2">

The following command will install Xfce.
<pre class="example">$ sudo yum install -y epel-release
$ sudo yum groupinstall -y "Xfce"
$ sudo reboot
</pre>
</div>
</div>
<div id="outline-container-sec-2" class="outline-2">
<h2 id="sec-2"><span class="section-number-2">2</span> Login to Xfce</h2>
<div id="text-2" class="outline-text-2">

You can select other desktop environment.
<div class="figure">

<img class="alignnone size-full wp-image-1529" src="http://hss5.com/wp-content/uploads/2018/11/0001_Login-1.png" width="1024" height="830" alt="0001_Login.png" />

</div>
Xfce is displayed.
<div class="figure">

<img class="alignnone size-full wp-image-1530" src="http://hss5.com/wp-content/uploads/2018/11/0002_Desktop-1.png" width="1024" height="830" alt="0002_Desktop.png" />

</div>
</div>
</div>
<div id="outline-container-sec-3" class="outline-2">
<h2 id="sec-3"><span class="section-number-2">3</span> Uninstall Xfce</h2>
<div id="text-3" class="outline-text-2">

The following command will uninstall Xfce.
<pre class="example">$ sudo yum groupremove -y "Xfce"
$ sudo yum remove -y libxfce4*
</pre>
</div>
</div>
</div>
</div>
<div id="cc-m-12871266292" class="j-module n j-spacing "></div>
</div>
</div>