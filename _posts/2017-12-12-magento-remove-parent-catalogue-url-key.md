---
ID: 1087
post_title: >
  magento去除子分类的url地址中带有父分类的url
  key
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/12/12/magento-remove-parent-catalogue-url-key/
published: true
post_date: 2017-12-12 15:41:56
---
app/code/core/Mage/Catalog/Model/Url.php 找到如下代码
方法getCategoryRequestPath
<blockquote>
<pre class="hljs php"><span class="hljs-keyword">if</span> (<span class="hljs-keyword">null</span> === <span class="hljs-variable">$parentPath</span>) {
   <span class="hljs-variable">$parentPath</span> = <span class="hljs-variable">$this</span>-&gt;getResource()-&gt;getCategoryParentPath(<span class="hljs-variable">$category</span>);
  }
   <span class="hljs-keyword">elseif</span> (<span class="hljs-variable">$parentPath</span> == <span class="hljs-string">'/'</span>) {
  <span class="hljs-variable">$parentPath</span> = <span class="hljs-string">''</span>;
   }


</pre>
</blockquote>
改成
<blockquote>
<pre class="hljs bash">//<span class="hljs-keyword">if</span> (null === <span class="hljs-variable">$parentPath</span>) {
   //<span class="hljs-variable">$parentPath</span> = <span class="hljs-variable">$this</span>-&gt;getResource()-&gt;getCategoryParentPath(<span class="hljs-variable">$category</span>);
 // }
  // elseif (<span class="hljs-variable">$parentPath</span> == <span class="hljs-string">'/'</span>) {
  <span class="hljs-variable">$parentPath</span> = <span class="hljs-string">''</span>;//这句不用注释
//   }

改完后，刷新下url索引就可以了


</pre>
</blockquote>