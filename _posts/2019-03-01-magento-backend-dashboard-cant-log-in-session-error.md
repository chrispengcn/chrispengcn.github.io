---
ID: 2432
post_title: 'magento backend dashboard can&#8217;t log in &#8211; session error'
author: chrispengcn
post_excerpt: |
  可能很多朋友有同样的经历，magento在服务器中配置域名是可以正常的访问了，但是在本地配置后却无法登录后台，账号密码登录的时候发现出现空白，无法跳转到后台，本文章向大家介绍两种解决本地magento后台无法登录的方法,需要的朋友可以参考下
layout: post
permalink: >
  http://hss5.com/2019/03/01/magento-backend-dashboard-cant-log-in-session-error/
published: true
post_date: 2019-03-01 17:55:40
---
<div class="art_desc mt10">
<div id="art_demo">可能很多朋友有同样的经历，magento在服务器中配置域名是可以正常的访问了，但是在本地配置后却无法登录后台，账号密码登录的时候发现出现空白，无法跳转到后台，本文章向大家介绍两种解决本地magento后台无法登录的方法,需要的朋友可以参考下</div>
</div>
<div class="lbd clearfix"></div>
<div id="content">

magento 后台无法登录解决办法

<strong>解决方法一：</strong>

这是一个cookie问题，使用firefox等非IE核心浏览器可以解决这个问题，虽然浏览器处理cookie的方式很相似但并不是100%相同， Magento其它的版本也有这个问题。

详细的修正这个问题的方法是定位到: app/code/core/Mage/Core/Model/Session/Abstract/Varien.php 。

大约在70行左右你可以看到类似的：
<div class="jb51code">
<pre class="brush:php;">// set session cookie params
/* 码农教程 http://www.manongjc.com */
session_set_cookie_params(
$this-&gt;getCookie()-&gt;getLifetime(),
$this-&gt;getCookie()-&gt;getPath() // 注释掉后面或删除
</pre>
</div>
<strong>解决方法二：</strong>

不用localhost登陆，

改为你的IP地址登陆：例如http://192.168.1.100/加后台地址，

也可以到apache里指向其它地址，

在服务器上一般不会出现这问题，不用修改。

<strong>magento1.9 后台无法登陆问题</strong>

打开 magento/app/code/core/Mage/Core/Model/Session/Abstract/varien.php

找到下面的代码，注释掉$cookieParams['domain'] = $cookie-&gt;getDomain();这行，就行了。
<div class="jb51code">
<pre class="brush:php;">if (isset($cookieParams['domain'])) {
$cookieParams['domain'] = $cookie-&gt;getDomain();
}
</pre>
</div>
结果如下
<div class="jb51code">
<pre class="brush:php;">if (isset($cookieParams['domain'])) {
//$cookieParams['domain'] = $cookie-&gt;getDomain();
}
</pre>
</div>
但是按照这个去做之后，还是出现错误，于是我把下面这段全部注释掉
<div class="jb51code">
<pre class="brush:php;">//if (isset($cookieParams['domain'])) {
//$cookieParams['domain'] = $cookie-&gt;getDomain();
// }

</pre>
</div>
感谢阅读，希望能帮助到大家，谢谢大家对本站的支持！

</div>