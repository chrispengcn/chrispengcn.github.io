---
ID: 1104
post_title: >
  magento
  立即购买跳转到结账页面
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/01/08/magento-%e7%ab%8b%e5%8d%b3%e8%b4%ad%e4%b9%b0%e8%b7%b3%e8%bd%ac%e5%88%b0%e7%bb%93%e8%b4%a6%e9%a1%b5%e9%9d%a2/'
published: true
post_date: 2018-01-08 17:16:22
---
<div class="exp-title clearfix">
<h1 title="magento 立即购买跳转到结账页面">magento 立即购买跳转到结账页面</h1>
</div>
<div id="format-exp" class="exp-content format-exp">
<div class="exp-content-block">
<div class="exp-content-body exp-brief-step">
<div class="exp-content-listblock">
<div class="content-listblock-text">

magento添加立即购买程序

</div>
</div>
</div>
</div>
<div class="exp-content-block">
<h2 class="exp-content-head"></h2>
<div class="audio-wp audio-wp-2" data-text="" data-for="" data-index="1"></div>
<div class="exp-content-body">
<ul class="exp-content-unorderlist ">
 	<li class="exp-content-list list-item-1">
<div class="content-list-text">apache+mysql+php</div></li>
</ul>
</div>
</div>
<div class="exp-content-block">
<h2 class="exp-content-head"><a name="section-3"></a></h2>
<div class="audio-wp audio-wp-2" data-text="" data-for="" data-index="2"></div>
<div class="exp-content-body">
<ol class="exp-conent-orderlist">
 	<li class="exp-content-list list-item-1">
<div class="list-icon">1</div>
<div class="content-list-text">

先找到该目录下的app\design\frontend\default\blank\template\catalog\product\view\

addtocart.phtml 文件

</div></li>
 	<li class="exp-content-list list-item-2">
<div class="list-icon">2</div>
<div class="content-list-text">

然后对这个文件做修改：

&lt;button id="checknow" type="button" title="&lt;?php echo $this-&gt;__('立即购买') ?&gt;" class="button btn-cart" &gt;

&lt;span&gt;

&lt;?php echo $this-&gt;__('立即购买') ?&gt;&lt;/span&gt;

&lt;/button&gt;

&lt;input type="hidden" name="return_url" id="return_url" value="" /&gt;

</div></li>
 	<li class="exp-content-list list-item-3">
<div class="list-icon">3</div>
<div class="content-list-text">

新增立即购买按钮，隐藏跳转地址。

再新增jQuery方法操作：

&lt;script&gt;

jQuery("#checknow").click(function(){

jQuery("input#return_url").val("&lt;?php echo $this-&gt;getUrl('checkout/onepage/'); ?&gt;");

productAddToCartForm.submit();

})

&lt;/script&gt;

OK了。这样你点击立即购买就能直接跳到结账页面。点击加入购物车跳到购物车页面

</div></li>
</ol>
</div>
</div>
</div>