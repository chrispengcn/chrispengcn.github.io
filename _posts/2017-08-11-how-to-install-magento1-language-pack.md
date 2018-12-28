---
ID: 952
post_title: >
  以安装中文语言包为例，介绍如何安装Magento多国语言安装包
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/08/11/how-to-install-magento1-language-pack/
published: true
post_date: 2017-08-11 09:30:35
---
<p id="best-content-1472568086" class="best-text mb-10">默认Magento仅仅有English语言包，所以如果想要显示其他语言，就要安装相应的语言包。 本文以安装Magento中文语言包为例，介绍如何安装多国语言包。</p>
<p class="best-text mb-10">Magento中文语言包安装步骤：</p>

<ul>
 	<li class="best-text mb-10">1. 首先，去 下载中文语言包，解压缩到Magento目录下，把它复制到两个目录: 一个是app\design\frontend\default\default\locale，另一个是app\locale。</li>
 	<li class="best-text mb-10">2. 去System - Configuration， 在页面左上角Current Configuration Scope下拉框处, 你能看见Default Config-Main Website-Main Website Store-Default Store View。</li>
 	<li class="best-text mb-10">3. 现在去System-Manage Stores，创建一个新的Store View: Store: Main Store Name: 中文 Code: chinese Status: Enabled Sort order: 0</li>
 	<li class="best-text mb-10">4. 存储后，回到System Configuration。在Current Configuration Scope下拉框，现在看到Chinese store view，点击： 在Locale options tab, 移除“use website” 检查框的选择然后把它的locale改为中Chinese (China)，存盘。 现在你就有中文的网店了 。</li>
</ul>
<p class="best-text mb-10"></p>
<p class="best-text mb-10">其他语言包的安装过程类似，在此不再赘述。</p>