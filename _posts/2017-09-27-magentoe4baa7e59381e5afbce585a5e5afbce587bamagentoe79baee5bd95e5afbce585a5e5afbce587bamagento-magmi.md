---
ID: 1058
post_title: >
  Magento产品导入导出|Magento目录导入导出|Magento
  Magmi
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2017/09/27/magento%e4%ba%a7%e5%93%81%e5%af%bc%e5%85%a5%e5%af%bc%e5%87%bamagento%e7%9b%ae%e5%bd%95%e5%af%bc%e5%85%a5%e5%af%bc%e5%87%bamagento-magmi/'
published: true
post_date: 2017-09-27 15:22:08
---
<div class="post-content">

捣鼓了几天关于<strong>Magento产品和目录的导出导入</strong>
，总结几点可行方法
<ol>
 	<li>使用外国佬开发的软件：<strong>Store Manager for Magento</strong>
, 免费版有10个产品导入数的限制，完全版需要199刀。。哪位朋友有<strong>Store Manager for Magento绿色版</strong>
或<strong>破解版</strong>
的话希望能共享下。。 嘿嘿。
使用Store Manager for Magento进行Magento产品导入导出和Magento目录导入导出是我认为最简单方便的办法。</li>
 	<li>使用Magento的开源项目：<strong>Magmi</strong>
，Magmi是让从CSV文件数据流导入导出产品和目录的脚本，功能相当给力，当前最新版本为 Magmi 0.6.17，使用很简单，只要将整个压缩包解压到magento网站根目录就行。通过http://domain/magmi/web/magmi.php这种方式访问。
关于Magmi的中文资料实在少，不得不在E文里滚来滚去。。
http://sourceforge.net/apps/phpWebSite/magmi/index.php
http://www.magentocommerce.com/boards/v/viewthread/201210/</li>
 	<li>第三种方法就比较手动繁琐了，流程如下
1，导出产品
admin &gt; import/export &gt; profiles &gt; export all products 获得产品的csv文件export_all_products.csv2，导出产品图片
将旧站\media\catalog\product\里的所有目录拷到新站的\media\import\这个目录里。

3，导出产品属性和属性集
操作数据库，将带eva_前缀的表导出一份到attribute_sets.sql文件中。确保在sql文件里的头部有加上SET FOREIGN_KEY_CHECKS=0;这句。

4，导出产品目录
同样操作数据库，导出catalog_category_前缀的表到categories.sql文件。

经过这4个步骤后，有如下
export_all_products.csv
在\media\import\的产品图片
attribute_sets.sql
categories.sql

5，导入产品
admin &gt; import/export &gt; profiles &gt; import all products 上传export_all_products.csv后执行run

6，导入属性集
运行attribute_sets.sql

7，导入目录
运行categories.sql
大概流程如此，期间可能问题7788。。各显神通了。</li>
</ol>
花了3 4 天都在搞<strong>Magento产品和目录的导出导入问题</strong>
，实在是相当疼(总站抽出单独站)。

关键是还没比较显著的成就。。匆匆总结以上3种Magento导入导出产品和目录的方法，哪位朋友也碰到这需求的一起探讨一下。

2011.3.15更：

目前已将总站抽出14个独立品牌站，网站管理和速度方面提高了一大截。

历时开发2个月测试1个月的仿Magento外贸商城系统也正式上线！数据从Magento站导入。告别Magento咯。

本文永久地址：<a href="https://sjolzy.cn/Magento-Product-Import-and-Export-Magento-directory-import-and-export-Magento-Magmi.html">https://sjolzy.cn/Magento-Product-Import-and-Export-Magento-directory-import-and-export-Magento-Magmi.html</a>

</div>