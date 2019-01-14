---
ID: 1078
post_title: magento porto 模板修改
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2017/10/18/magento-porto-%e6%a8%a1%e6%9d%bf%e4%bf%ae%e6%94%b9/'
published: true
post_date: 2017-10-18 16:42:38
---
<blockquote>//去掉 search 框中的分类选择

.search-area .form-search select#cat {display:none;}

.header .form-search {padding-right:30px;}

.header-container.type21 .header #search_mini_form {
width: 300px;
}

&nbsp;

//产品卖点样式
.details-area .product-sku {font-weight:bold;}
.details-area .product-kvm-salepoint, .product-kvm-salepoint {font-weight:normal; color:#ce150c; }

.header-container .welcome-msg {float:left;}

porto 模板去掉 google 字体

appdesignfrontendsmartwaveportotemplatepagehtmlhead.phtml

line:35

li.menu-item a.level1 {font-weight:normal}</blockquote>
添加产品卖点

view.phtml   &amp;  list.phtml

&nbsp;

&lt;div class="details-area"&gt;
&lt;h2 class="product-name"&gt;&lt;a href="&lt;?php%20echo%20$_product-&gt;getProductUrl()%20?&gt;" title="&lt;?php echo $this-&gt;stripTags($_product-&gt;getName(), null, true) ?&gt;"&gt;&lt;?php echo $_helper-&gt;productAttribute($_product, $_product-&gt;getName(), 'name') ?&gt;&lt;/a&gt;&lt;/h2&gt;
&lt;p class="product-sku"&gt;&lt;?php echo $this-&gt;__('Sku:'); ?&gt;&lt;?php echo $_product-&gt;getSku(); ?&gt;&lt;/p&gt;

&lt;?php if($_product-&gt;getKvm_salepoint() ): ?&gt;
&lt;p class="product-kvm-salepoint"&gt;
&lt;?php echo $this-&gt;htmlEscape($_product-&gt;getKvm_salepoint()) ?&gt;
&lt;/p&gt;
&lt;?php endif; ?&gt;

&nbsp;