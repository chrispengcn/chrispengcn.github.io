---
ID: 198
post_title: >
  magento搬家 magento换服务器步骤
  更改数据库连接 更换域名
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/08/31/magento%e6%90%ac%e5%ae%b6-magento%e6%8d%a2%e6%9c%8d%e5%8a%a1%e5%99%a8%e6%ad%a5%e9%aa%a4-%e6%9b%b4%e6%94%b9%e6%95%b0%e6%8d%ae%e5%ba%93%e8%bf%9e%e6%8e%a5-%e6%9b%b4%e6%8d%a2%e5%9f%9f%e5%90%8d/'
published: true
post_date: 2013-08-31 06:46:22
---
<h3>&nbsp;</h3> <ul> <li>Last update on 2013 年 8 月 26 日  <li>under <a href="http://www.51baofeng.com/wiki/archives/category/other-faq/magento-migration">magento搬家</a></li></ul> <p>1、把magento的目录复制到新服务器，把数据库导出，导入。 <p>如果导不进去的是因为magento的数据库使用了外键约束，通过phpmyadmin导入的时候会报错，在导出的sql文件上加一行 <p>SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0; <p>2、修改magento的配置文件，位置在app/etc/local.xml，注意修改CDATA里面的内容 主要是数据库连接 数据库用户名 密码 数据库名称 <p>3、修改magento数据库，core_config_data表中的path为web/unsecure/base_url和web/secure/base_url的内容，为你网站的新域名，注意域名后面的“/”。更换网站完整域名+/&nbsp; http://www.abc.com/ <p>一切OK，如果不好使可以进到后台刷新一下缓存，重建一下索引</p>