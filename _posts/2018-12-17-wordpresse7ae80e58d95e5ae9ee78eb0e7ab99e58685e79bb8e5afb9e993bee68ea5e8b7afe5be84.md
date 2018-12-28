---
ID: 1747
post_title: >
  WordPress简单实现站内相对链接路径
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/17/wordpress%e7%ae%80%e5%8d%95%e5%ae%9e%e7%8e%b0%e7%ab%99%e5%86%85%e7%9b%b8%e5%af%b9%e9%93%be%e6%8e%a5%e8%b7%af%e5%be%84/'
published: true
post_date: 2018-12-17 10:52:27
---
<div class="cxtheme-single-header"></div>
<div class="cxtheme-single-main">
<div id="article-index">

前几天在用户提出了一个需求，希望在自己的站内使用相对地址，但是呢在网上找的教程要么太复杂需要修改WP程序的源代码要么就是没有什么卵用，于是来向我求助有没有简单有效的方案来使用相对地址，再然后就有了今天的这篇帖子给大家带来一种非常简单，而且非常有效的方法，并非网络上那些修改修改wp源文件或者wp-config.php配置文件这些治标不治本的方案，而是wp源码中内置的功能只需做下简单调用即可！

</div>
<div class="post-countent-data">
<p class="main-p-imgbox"><img class="alignnone size-full wp-image-1750" src="http://hss5.com/wp-content/uploads/2018/12/201609257568670-1.jpg" width="1000" height="585" alt="201609257568670" /></p>
关于相对路径和绝对路径的解释和优缺点分析呢我已经在《绝对地址和相对地址的优缺点分析和使用建议》中进行了详细的说明。通过这篇文章我们可以链接到什么情况下适合用相对链接，什么时候用绝对链接比较合适！下面我们就来看看如何通过简单的一行代码来实现WP站内相对链接的功能的！
<h2 id="title-0">方法分析</h2>
要想简单的实现相对链接无非就是找到代码的源头，然后再源头进行匹配和替换，那么对于WP来说，我们的源头是在常规里面填写的网址，但是这个我们是不能去修改的，值得庆幸的是WP在一般情况下是没有直接调用常规里面设置的网址的，而是通过一个home_url() 的函数进行应用，我们要做的就是在home_url()这的函数的返回值中进行匹配和替换，如果跟网站域名相同那么就把域名去除，如果不同则直接返回网址，这样在兼容附件服务器的同时实现了站内相对链接的功能！
<h2 id="title-1">实现代码</h2>
实现相对链接功能，我们需要用到home_url()函数中提供的一个home_url过滤器，和WP内置的wp_make_link_relative函数来匹配替换跟主域相同的域名：
<div class="dp-highlighter nogutter">
<ol class="dp-j" start="0">
 	<li class="alt">add_filter( ‘home_url’, ‘wp_make_link_relative’ );</li>
</ol>
</div>
这个时候我们可能会发现一些问题，sitemap与feed中也调用相对链接那么站外访问和搜索引擎抓取就会报错，下面我们需要对上面的代码进行优化：
<div class="dp-highlighter nogutter">
<ol class="dp-j" start="0">
 	<li class="alt">add_filter( ‘home_url’, ‘cx_remove_root’ );</li>
 	<li class="">function cx_remove_root( $url ) {</li>
 	<li class="alt">    <span class="keyword">if</span>(!is_feed() || !get_query_var( ‘sitemap’ )){</li>
 	<li class="">        $url = preg_replace( ‘|^(https?:)?<span class="comment">//[^/]+(/?.*)|i’, ‘$2’, $url );</span></li>
 	<li class="alt">        <span class="keyword">return</span> ‘/’ . ltrim( $url, ‘/’ );</li>
 	<li class="">    }<span class="keyword">else</span>{</li>
 	<li class="alt">        <span class="keyword">return</span> $url;</li>
 	<li class="">    }</li>
 	<li class="alt">}</li>
</ol>
</div>
<blockquote class="geshi"><i class="iconfont layui-extend-quote"></i>这段代码兼容性就比较好了，sitemap与feed都可以继续使用绝对链接；这种方法相对来说比较方便和安全！</blockquote>
<h2 id="title-2">注意事项</h2>
<ol>
 	<li>本教程只适用于代码比较规范的主题，不保证兼容所有主题；</li>
 	<li>教程中的代码添加到主题的functions.php文件中即可；</li>
</ol>
</div>
</div>