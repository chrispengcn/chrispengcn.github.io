---
ID: 3586
post_title: >
  简单方法实现WordPress多个域名绑定和访问
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'https://www.hss5.com/2020/05/25/%e7%ae%80%e5%8d%95%e6%96%b9%e6%b3%95%e5%ae%9e%e7%8e%b0wordpress%e5%a4%9a%e4%b8%aa%e5%9f%9f%e5%90%8d%e7%bb%91%e5%ae%9a%e5%92%8c%e8%ae%bf%e9%97%ae/'
published: true
post_date: 2020-05-25 10:14:16
---
今天这个客户需要将20个新注册的域名全部指向到一个目录中且搭建一个WordPress程序，且需要每个域名打开都能看到独立的WordPress，而且在页面中不会串到其他域名。理论上我们在安装WordPress之后域名都是唯一性的，甚至WWW和不带WWW都是唯一且会自动跳转。

默认情况下肯定不可以实现多个域名绑定实现独立的访问，即便可以打开，如果点击WordPress网站中的页面、内容链接肯定会跳转到当初安装站点默认配置的域名。那如何实现呢？继续找啊找。
<h2 id="title-0">第一、实现任意域名的访问</h2>
<blockquote>define('WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST']);
define('WP_HOME', 'http://' . $_SERVER['HTTP_HOST']);</blockquote>
我们在WordPress程序根目录wp-config.php文件中加上上面代码，这样只要是解析进来的域名都可以打开且不会看到串联到其他域名。
<h2 id="title-1">第二、限制特定域名访问</h2>
<blockquote>$domain = array("www.itbulu.com", "www.laobuluo.com", "www.laojiang.me");
if(in_array($_SERVER['HTTP_HOST'], $domain)){
define('WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST']);
define('WP_HOME', 'http://' . $_SERVER['HTTP_HOST']);
}</blockquote>
这样的配置就是限制设置的域名才可以访问，其他都无法访问。这样可以实现WordPress多个域名绑定和访问。