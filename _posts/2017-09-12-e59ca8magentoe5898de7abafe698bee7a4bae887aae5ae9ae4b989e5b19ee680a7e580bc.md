---
ID: 1033
post_title: 在magento前端显示自定义属性值
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2017/09/12/%e5%9c%a8magento%e5%89%8d%e7%ab%af%e6%98%be%e7%a4%ba%e8%87%aa%e5%ae%9a%e4%b9%89%e5%b1%9e%e6%80%a7%e5%80%bc/'
published: true
post_date: 2017-09-12 09:32:03
---
magento有强大的自定义属性的功能，允许用户对产品进行各种属性定义。在这里记录下开发过程中遇到的各种属性值的前端输出。

1. 简单文本 ，前端显示代码如下：

$_product-&gt;getAttributeName()

其中AttributeName为属性字段，比如你自定义了字段color，则用$_product-&gt;getColor()  获取

2. 文本区域（可嵌入html代码），比如certificate字段为添加图文描述，前端显示certificate字段获取代码如下：
<blockquote>&lt;?php <span class="vars">$_description</span> = <span class="vars">$_product</span>-&gt;getCertificate(); ?&gt;

&lt;?php <span class="func">echo</span> <span class="vars">$this</span>-&gt;helper(<span class="string">'catalog/output'</span>)-&gt;productAttribute(<span class="vars">$this</span>-&gt;getProduct(), <span class="vars">$_description</span>, <span class="string">'certificate'</span>) ?&gt;</blockquote>
3. 下拉框,比如获取color下拉框字段值
<blockquote>$_product-<span class="tag">&gt;</span>getResource()-<span class="tag">&gt;</span>getAttribute('color')-<span class="tag">&gt;</span>getFrontend()-<span class="tag">&gt;</span>getValue($_product)</blockquote>
&nbsp;