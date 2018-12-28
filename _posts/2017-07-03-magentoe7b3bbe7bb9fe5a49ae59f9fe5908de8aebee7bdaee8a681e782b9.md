---
ID: 831
post_title: Magento系统多域名设置要点
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2017/07/03/magento%e7%b3%bb%e7%bb%9f%e5%a4%9a%e5%9f%9f%e5%90%8d%e8%ae%be%e7%bd%ae%e8%a6%81%e7%82%b9/'
published: true
post_date: 2017-07-03 13:00:02
---
Magento支持多域多店设置，使用一个后台管理多个域方便了管理，它的原理不复杂，根据过来的HOST决定使用哪个域。
不过需要首先理解Website、Store、Store View的概念，如果能从数据库设计上理解的话，这个多域设置是最清楚的。
首先到System/Manage Stores创建一个Website:

注意这里为这个网站设置的代码为mgt，然后为这个网站设置一个Store。

注意这里需要选择Root Category，这意味着需要首先创建针对这个域的根目录。接下来为这个Store创建一个Store View。

注意这里也有一个针对Store View的code。
完成之后去到System/configuration，首先选择左上角的Current Configuration Scrop下拉选择刚刚创建的Website，然后选择左边Configuration下面的GENERAL的Web,修改如下两项：

Magento多域设置

注意把全部的右边对应的勾去掉，然后保存，这样在Magento后台设置的内容就完成了。
 
接下来去到Apache的虚拟机配置文件中：

Magento多域Apache虚拟机设置

注意这里使用了SetEnv指令配置环境变量，一个是MAGE_RUN_CODE，一个是MAGE_RUN_TYPE，当MAGE_RUN_TYPE值是website时，MAGE_RUN_CODE对应的是创建Website时输入的code，当MAGE_RUN_TYPE值是store时，MAGE_RUN_CODE对应的是创建Store view时输入的code。
 
一般以上方式的设置比较实用，不过如果你使用的是cpanel之类的虚拟主机，就是无法修改配置文件，那只能依靠<strong>.htaccess</strong>来设置了。
SetEnvIf Host .*magentoo.net MAGE_RUN_CODE=mgt
SetEnvIf Host .*magentoo.net MAGE_RUN_TYPE=website   # 'website' or 'store'

完成之后修改一下本地的HOST文件，让magentoo.net指向本地地址127.0.0.1（网站运行在本地的时候）。
访问http://magento.net成功，多域设置完成。


SetEnvIf Host magento.kinankvm.com MAGE_RUN_CODE=base
SetEnvIf Host magento.kinankvm.com MAGE_RUN_TYPE=website   # 'website' or 'store'

SetEnvIf Host m.szkinan.com MAGE_RUN_CODE=szkinan
SetEnvIf Host m.szkinan.com MAGE_RUN_TYPE=website   # 'website' or 'store'