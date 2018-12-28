---
ID: 987
post_title: 'magento 1.9  phtml 调用  静态区块'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/08/17/magetno1-9-call-static-block/
published: true
post_date: 2017-08-17 16:40:55
---
magento 1.9  phtml 调用  静态区块

在后台添加   static block,  名称  headblock

phtml 中插入
<blockquote>&lt;?php echo $this-&gt;getLayout()
-&gt;createBlock('cms/block')
-&gt;setBlockId('headblock')
-&gt;toHtml(); ?&gt;</blockquote>