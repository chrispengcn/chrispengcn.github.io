---
ID: 1266
post_title: >
  magento 2.x
  中css，字体，js等文件的404问题（图片图标字体css缺失）
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/07/06/magento-2-x-%e4%b8%adcss%ef%bc%8c%e5%ad%97%e4%bd%93%ef%bc%8cjs%e7%ad%89%e6%96%87%e4%bb%b6%e7%9a%84404%e9%97%ae%e9%a2%98%ef%bc%88%e5%9b%be%e7%89%87%e5%9b%be%e6%a0%87%e5%ad%97%e4%bd%93css%e7%bc%ba/'
published: true
post_date: 2018-07-06 17:59:05
---
<div class="lists"></div>
<div class="post_content">

两个可能原因：
1、权限：
$ rm -rf pub/static/*
$ php bin/magento setup:static-content:deploy

2、When Magento 2 is not in production mode, it will try to create symlinks for some static resources on local server. We have to change that behavior of Magento 2 by going to edit ROOT &gt; app &gt; etc &gt; di.xml file. Open up di.xml in your favorite code editor, find the virtualType name=”developerMaterialization” section. In that section below, you will find an item which needs to be modified. You can modify it by changing the following content:

Magento\Framework\App\View\Asset\MaterializationStrategy\Symlink
To:

Magento\Framework\App\View\Asset\MaterializationStrategy\Copy
Now last step, also delete old files generated in ROOT &gt; pub &gt; static &gt; DELETE ALL EXCEPT .HTACCESS

Clear / Flush Magento cache by typing “php bin/magento cache:flush” in CMD.

And finally, to Reindex Magento Static Blocks type “php bin/magento indexer:reindex”.

You are done with successful installation of Magento 2.

</div>