---
ID: 1051
post_title: magento 产品页和列表页显示sku
author: chrispengcn
post_excerpt: |
  magento 产品页和列表页显示sku
  magento 1.93 +  porto theme 2.6 测试成功
layout: post
permalink: >
  http://hss5.com/2017/09/27/magento-display-sku-in-product-page/
published: true
post_date: 2017-09-27 11:46:08
---
引用 http://www.hellokeykey.com/magento-display-sku/

&nbsp;
<blockquote>magento 产品页和列表页显示sku
magento 1.93 +  porto theme 2.6 测试成功

2017-9-27</blockquote>
1.在你的产品详细页你想显示SKU，代码如下

产品详细页文件为template\catalog\product\view.phtml
<div>
<div id="highlighter_278295" class="syntaxhighlighter php">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php </code><code class="php functions">echo</code> <code class="php variable">$this</code><code class="php plain">-&gt;htmlEscape(</code><code class="php variable">$_product</code><code class="php plain">-&gt;getSku()) ?&gt;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
我们给修饰一下，代码和效果如下
<blockquote>&lt;p&gt;&lt;span&gt;&lt;?php echo$this-&gt;__('Sku:') ?&gt;&lt;/span&gt;&lt;?php echonl2br($_product-&gt;getSku()) ?&gt; &lt;/p&gt;</blockquote>
&nbsp;
<blockquote><em>//注意  $_product-&gt;getAttributeName()</em>

<em>//getAttributeName （)函数， 首字母大写</em></blockquote>
&nbsp;

&nbsp;
<div class="container">
<div class="line number1 index0 alt2">

<img src="http://hss5.com/wp-content/uploads/2017/09/code-sku.gif" alt="magento sku" width="600" height="136" />

<img src="http://hss5.com/wp-content/uploads/2017/09/sku-view.gif" alt="magento sku" width="600" height="200" />

</div>
2.在产品的列表你想显示sku

产品列表也文件为template\catalog\product\list.phtml

代码如下
<div>
<div id="highlighter_121408" class="syntaxhighlighter php">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php </code><code class="php functions">echo</code><code class="php variable">$_product</code><code class="php plain">-&gt;getSku(); ?&gt;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<code>我们给修饰下，代码以及效果如下</code>

</div>
<div class="container">
<blockquote>
<div class="line number1 index0 alt2"><code class="php plain">&lt;p&gt;&lt;?php echo$this-&gt;__('Sku:'); ?&gt;&lt;?php echo$_product-&gt;getSku(); ?&gt;&lt;/p&gt;</code><code class="php plain"></code></div></blockquote>
</div>
&nbsp;
<div class="container">

<img src="http://hss5.com/wp-content/uploads/2017/09/list-sku.gif" alt="magento sku" width="600" height="125" />

<img src="http://hss5.com/wp-content/uploads/2017/09/list-sku-view.gif" alt="magento sku" />

</div>
&nbsp;

Example: 添加产品卖点
<blockquote>   &lt;div class="details-area"&gt;
&lt;h2 class="product-name"&gt;&lt;a href="&lt;?php echo $_product-&gt;getProductUrl() ?&gt;" title="&lt;?php echo $this-&gt;stripTags($_product-&gt;getName(), null, true) ?&gt;"&gt;&lt;?php echo $_helper-&gt;productAttribute($_product, $_product-&gt;getName(), 'name') ?&gt;&lt;/a&gt;&lt;/h2&gt;
&lt;p class="product-sku"&gt;&lt;?php echo $this-&gt;__('Sku:'); ?&gt;&lt;?php echo $_product-&gt;getSku(); ?&gt;&lt;/p&gt;

&lt;?php if($_product-&gt;getKvm_salepoint() ): ?&gt;
&lt;p class="product-kvm-salepoint"&gt;
&lt;?php echo $this-&gt;htmlEscape($_product-&gt;getKvm_salepoint()) ?&gt;
&lt;/p&gt;
&lt;?php endif; ?&gt;</blockquote>