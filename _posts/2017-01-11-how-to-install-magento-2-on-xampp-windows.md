---
ID: 715
post_title: >
  How to install Magento 2 on XAMPP
  windows?
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/01/11/how-to-install-magento-2-on-xampp-windows/
published: true
post_date: 2017-01-11 20:16:13
---
http://www.magentech.com/magento-2/useful-articles/item/588-how-to-install-magento-2-on-xampp
<div id="fb-root" class=" fb_reset"></div>
&nbsp;
<div id="k2Container" class="item-blogsmartaddons itemView">
<div class="itemHeader">
<h2 class="itemTitle">How to install Magento 2 on XAMPP?</h2>
</div>
<div class="itemToolbar"></div>
<div class="itemRatingBlock"></div>
<div class="itemBody"><span class="itemAuthor">Written by  <a href="http://www.magentech.com/magento-2/useful-articles/itemlist/user/36881-sarah">Sarah</a> </span>
<span class="itemDateCreated">Thursday, 24 December 2015 10:06</span>

<hr />

<div class="itemIntroText">

After the long-awaited time,<b> Magento 2.0 </b>has been finally released. It is considered a remarkable version with numerous changes compare to the previous version. Why don't we find out how to install the Magento 2 software?

<img title="Magento 2" src="http://hss5.com/wp-content/uploads/2017/01/magento-2-installation.jpg" alt="Magento 2" border="0" />

There are several ways to install Magento 2, and all of them offer their own benefits. But in this post we would like to bring to you a step by step guide to install Magento 2.0 with sample data on your local server.

</div>
<div class="itemFullText">
<h3>1. Install XAMPP</h3>
The first steps lead us to the installation guide page. It is highly important to check whether your server suits minimal Magento 2 requirements:
<ul>
 	<li><a href="http://devdocs.magento.com/guides/v1.0/install-gde/prereq/apache.html">Apache</a>: 2.2 or 2.4</li>
 	<li><a href="http://devdocs.magento.com/guides/v1.0/install-gde/prereq/php-ubuntu.html">PHP</a>: 5.4.11 or 5.5.x</li>
 	<li><a href="http://devdocs.magento.com/guides/v1.0/install-gde/prereq/mysql.html">MySQL</a>: 5.6.x</li>
 	<li>Required PHP extensions: PDO/MySQL, mbstring, mcrypt, mhash, SimpleXML, curl, xsl, gd2, ImageMagick 6.3.7 (or later) or both, soap, intl</li>
</ul>
<h3>2. Install Composer</h3>
Download<strong> <a href="https://getcomposer.org/Composer-Setup.exe" target="_blank" rel="nofollow">Composer-Setup.exe</a> </strong>and install on local

Run <strong>Composer-Setup.exe </strong>file

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-install-composer.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-install-composer.png" alt="composer setup" /></a>

&gt;&gt; Click on<strong> NEXT</strong> to continue

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-setup-composer.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-setup-composer.png" alt="composer setup" /></a>

&gt;&gt; Click on <strong>NEXT</strong> to continue

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-setting-composer.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-setting-composer.png" alt="composer setup" /></a>

&gt;&gt; Select the path where php.exe is located with xampp:<strong> C:/xampp/php/php.exe</strong> and then click on <strong>Next </strong>button

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-ready.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-ready.png" alt="composer installation" /></a>

&gt;&gt; Choose <strong>Install</strong>

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-installing.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-installing.png" alt="composer installation" /></a>

&gt;&gt;<strong> Finish</strong>: If you see the window is the same as the following one, you installed Composer successfully.

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-complete.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-complete.png" alt="composer installation" /></a>

&gt;&gt; If you are facing with the problem related to <a href="http://images.smartaddons.com/smartaddons/images/blog/magentech/Installing-magento2/8_error.png" target="_self">shortage of media</a>, please run the command: <b><i>php bin/magento setup:static-content:deploy</i></b>

<a href="http://hss5.com/wp-content/uploads/2017/01/comand_deploy.png"><img src="http://hss5.com/wp-content/uploads/2017/01/comand_deploy.png" alt="composer installation" /></a>

&gt;&gt; Once completed, you should get this screen

<a href="http://hss5.com/wp-content/uploads/2017/01/deploy_complete.png"><img src="http://hss5.com/wp-content/uploads/2017/01/deploy_complete.png" alt="composer installation" /></a>
<h3>3. Install Magento 2.0</h3>
&gt;&gt; Get <b>Magento 2.0</b> from Magento: <a href="https://github.com/magento/magento2">https://www.magentocommerce.com/download</a>. Alternatively you can download Magento 2 <a href="http://magentech.com/news/item/577-magento-community-edition-20-is-ready" target="_self">here</a>.

&gt;&gt; Next, Log into <strong>MySQL</strong> to create database

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-install-database.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-install-database.png" alt="create database" /></a>

&gt;&gt; Then, open browser and go to: <b>http://localhost/magento/magento20/</b> to start install Magento 2.0

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-agree.jpg"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-agree.jpg" alt="install magento 2 step 1" /></a>
<h4>Installation Run:</h4>
Follow this below steps to run Installation process
<h4>Step 1</h4>
&gt;&gt; Choose <strong>Start Readiness Check</strong>. After Start Readiness Check is completed, select <strong>Next</strong>

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-installation.jpg"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-installation.jpg" alt="install magento 2 step 1.1" /></a>

&gt;&gt; you need to click on "Start Readiness Check". It maybe occur the error like the below image

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-installation2.1.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-installation2.1.png" alt="install magento 2 step 2" /></a>

&gt;&gt; You need to open your php.ini file and set always_populate_raw_post_data to -1.

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-installation2.2.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-installation2.2.png" alt="install magento 2 step 2" /></a>
<h4>Step 2</h4>
&gt;&gt; Enter server and database in step 2 and then click on<strong> Next</strong>

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-step2.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-step2.png" alt="magento2-install-step4" /></a>
<h4>Step 3</h4>
&gt;&gt; Put your website link and continue to click on<strong> Next</strong>

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-install-step3.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-install-step3.png" alt="magento2-install-step5" /></a>
<h4>Step 4</h4>
&gt;&gt; Customize your store: you can choose time <strong>Zone, Currency and Languages</strong>

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-install-step4.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-install-step4.png" alt="magento 2 install step 4" /></a>
<h4>Step 5</h4>
&gt;&gt; You need to <strong>Create Admin Account</strong>. Enter your information and move to the next step

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-install-step5.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-install-step5.png" alt="magento2-install-step6" /></a>
<h4>Step 6</h4>
&gt;&gt; In this step, you should click on the button: <strong>Install Now</strong>

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-install-step6.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-install-step6.png" alt="magento 2 installation" /></a>

&gt;&gt; It will take some minutes to complete this installation process. If your window is the same as below, <strong>CONGRAT</strong>! You install Magento 2.0 successfully

<a href="http://hss5.com/wp-content/uploads/2017/01/magento2-finish.png"><img src="http://hss5.com/wp-content/uploads/2017/01/magento2-finish.png" alt="magento2-install-step9" /></a>

If you have any queries, please send us your questions, our Magento expert will support you in any single step.

<i>Thanks for reading!</i>

</div>
</div>
</div>
&nbsp;