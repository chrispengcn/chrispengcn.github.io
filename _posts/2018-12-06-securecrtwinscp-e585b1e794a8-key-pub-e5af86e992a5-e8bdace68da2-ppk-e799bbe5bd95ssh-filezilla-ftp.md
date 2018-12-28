---
ID: 1660
post_title: 'SecureCRT+WinSCP 共用 key pub 密钥 转换 ppk 登录ssh  &#8211; filezilla ftp'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/06/securecrtwinscp-%e5%85%b1%e7%94%a8-key-pub-%e5%af%86%e9%92%a5-%e8%bd%ac%e6%8d%a2-ppk-%e7%99%bb%e5%bd%95ssh-filezilla-ftp/'
published: true
post_date: 2018-12-06 22:29:11
---
<div class="artical-title-list"><a class="time fr">2012-09-29 10:19:13</a>
<div class="clear"></div>
</div>
<div class="artical-content-bak main-content editor-side-new">
<div id="result" class="con editor-preview-side">

来源:<a href="http://www.weiruoyu.cn/?p=325" target="_blank" rel="noopener">http://www.weiruoyu.cn/?p=325</a>

使用SecureCRT生成的密钥，无法在WinSCP使用，

使用puttygen.exe无法直接转换，解决办法

1.使用大于等于SecureCRT6.5版本，来转换

<img class="alignnone size-full wp-image-1661" src="http://hss5.com/wp-content/uploads/2018/12/101125908.jpg" width="558" height="236" alt="1" />

<img class="alignnone size-full wp-image-1662" src="http://hss5.com/wp-content/uploads/2018/12/101125679.jpg" width="372" height="139" alt="1" />

记得放入私钥，不是pub公钥。然后保存到桌面OpenSSH格式的密钥

<img class="alignnone size-full wp-image-1663" src="http://hss5.com/wp-content/uploads/2018/12/101125441.jpg" width="571" height="436" alt="1" />

注：英文版本使用：

SecureCRT:Quick Connect-&gt; Authentiation -&gt; Public Key -&gt; Properties -&gt;Create Identity File -&gt; DSA/RSA -&gt; Set Passphrase -&gt; Done

2.使用puttygen.exe转换 ppk格式密钥

<img class="alignnone size-full wp-image-1664" src="http://hss5.com/wp-content/uploads/2018/12/101618165.jpg" width="481" height="462" alt="101618165.jpg" />

放入OpenSSH的私钥，并输入密码

<img class="alignnone size-full wp-image-1665" src="http://hss5.com/wp-content/uploads/2018/12/101618757.jpg" width="493" height="477" alt="101618757.jpg" />

<img class="alignnone size-full wp-image-1666" src="http://hss5.com/wp-content/uploads/2018/12/102409818.jpg" width="493" height="477" alt="102409818.jpg" />

3.使用WinSCP使用ppk密钥即可登录

<strong>下载附件puttygen.exe：<a href="http://www.weiruoyu.cn/?p=325" target="_blank" rel="noopener">http://www.weiruoyu.cn/?p=325</a></strong>

来源：<a href="http://hi.baidu.com/bailiangcn/item/14a8cbd6318bd62a39f6f706" target="_blank" rel="noopener">http://hi.baidu.com/bailiangcn/item/14a8cbd6318bd62a39f6f706</a>

</div>
</div>
<div class="filedown">附件：<a href="http://down.51cto.com/data/2361516" target="_blank" rel="noopener">http://down.51cto.com/data/2361516</a></div>