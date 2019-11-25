---
ID: 3342
post_title: 'Install: Quick Install Magento 2 on litespeed'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/11/25/install-quick-install-magento-2-on-litespeed/
published: true
post_date: 2019-11-25 18:20:27
---
<h2><span id="Install_Quick_Install_Magento_2" class="ez-toc-section">Install: Quick Install Magento 2</span></h2>
After you <a href="https://openlitespeed.org/knowledge-base/1-click-install/">install OpenLiteSpeed</a>, follow the instructions below to get your Magento 2 site working.

You MUST <b>enable Rewrites</b> and “<b>Auto Load from .htaccess</b>” also. Magento 2 relies on .htaccess to work properly. Assuming your PHP, Script Handler, log and other options are already set, you can now save everything and reload OpenLiteSpeed by clicking on the <b>Graceful Restart</b> button.
<h3><span id="Run_the_Magento_Install_Script" class="ez-toc-section"><a id="RuntheMagentoinstallscript"></a>Run the Magento Install Script</span></h3>
<h4><span id="Step_1_Run_Installation_Script" class="ez-toc-section">Step 1. Run Installation Script</span></h4>
Point your browser with default port to https://yourstore.com/
Accept terms and conditions by clicking ‘Accept and Setup Magento’.

<img class="alignnone size-full wp-image-3343" src="https://www.hss5.com/wp-content/uploads/2019/11/magento2-1-300x253.png" alt="" width="300" height="253" />
<h4><span id="Step_2_Readiness_Check" class="ez-toc-section">Step 2. Readiness Check</span></h4>
<div class="level3">

<img class="alignnone size-full wp-image-3344" src="https://www.hss5.com/wp-content/uploads/2019/11/readiness_check-1024x462.png" alt="" width="1024" height="462" />

</div>
Magento CE installer needs to check if all the requirements are met. If you have followed all the guidelines above, all the requirements should be met, so just click ‘Start Readiness Check’.

<img class="alignnone size-full wp-image-3345" src="https://www.hss5.com/wp-content/uploads/2019/11/magento2-3-1024x567.png" alt="" width="1024" height="567" />

&nbsp;
<h4 id="set_up_database" class="sectionedit11"><span id="Step_3_Set_up_Database" class="ez-toc-section">Step 3. Set up Database</span></h4>
We’ve created the database above. Just enter the database details and click <strong>Save and continue</strong>. If the Magento installer is successfully able to connect to the database, it will start the installation process.

Enter the database details we created above.

<img class="alignnone size-full wp-image-3346" src="https://www.hss5.com/wp-content/uploads/2019/11/magento-database-1024x591.png" alt="" width="1024" height="591" />
<h4 id="configure_site" class="sectionedit12"><span id="Step_4_Web_Configuration" class="ez-toc-section">Step 4. Web Configuration</span></h4>
<div class="level3"></div>
Set up your Store and Admin URL Path, e.g.
<ul>
 	<li class="level1">
<div class="li">Your Store Address: <a class="urlextern" title="http://yourdomain.com" href="http://yourdomain.com/" rel="nofollow">http://yourdomain.com/</a></div></li>
 	<li class="level1">
<div class="li">Magento Admin Address: <a class="urlextern" title="http://yourdomain.com/admin" href="http://yourdomain.com/admin" rel="nofollow">http://yourdomain.com/admin</a></div></li>
</ul>
<img class="alignnone size-full wp-image-3347" src="https://www.hss5.com/wp-content/uploads/2019/11/magento-web-configurations-1024x544.png" alt="" width="1024" height="544" />
<h4><span id="Step_5_Create_Admin_Account" class="ez-toc-section">Step 5. Create Admin Account</span></h4>
<div class="level3">

On this step, the installer will let you configure settings for your site. Example settings are:
<ul>
 	<li class="level1">
<div class="li"><strong>Site name:</strong> <code>Litespeedtech</code></div></li>
 	<li class="level1">
<div class="li"><strong>Site email address:</strong> <code>magemto@example.com</code></div></li>
 	<li class="level1">
<div class="li"><strong>Username:</strong> <code>litespeedtech</code></div></li>
 	<li class="level1">
<div class="li"><strong>Password:</strong> <code>litespeedtech</code></div></li>
</ul>
</div>
<h4><span id="Step_6_Install" class="ez-toc-section">Step 6. Install</span></h4>
<img class="alignnone size-full wp-image-3348" src="https://www.hss5.com/wp-content/uploads/2019/11/magento2-5-1024x324.png" alt="" width="1024" height="324" />
<h3></h3>
<h3><span id="Good_Practice_Change_document_root" class="ez-toc-section"><a id="changedocroot"></a>Good Practice: Change document root</span></h3>
<ol>
 	<li>IMPORTANT NOTE: Magento 2 should never run on its root file structure on a production server like we just configured. After installing it, you must set its Document Root to /home/magento2/public_html/pub/, or else you can have a lot of problems with redirect loops, security, etc. No matter what file structure you are using, Magento Document Root must always point to “your-file-structure/pub” after installing.</li>
</ol>
&nbsp;
<ol>
 	<li>Setting the webroot to the pub/ directory prevents site visitors from accessing the Web Setup Wizard and other sensitive areas of the Magento file system from a browser.
If you’re accustomed to using the Web Setup Wizard during development, be aware that you will not be able to access it when serving files from the pub/ dir.</li>
 	<li>Please refer to <a href="https://devdocs.magento.com/guides/v2.3/install-gde/tutorials/change-docroot-to-pub.html">this Magento 2 office document for doc root recommendation</a>.
<ol>
 	<li>
<h3><span id="Cannot_use_LiteMage_on_OLS" class="ez-toc-section">Cannot use LiteMage on OLS</span></h3>
Although the LiteMage cache plugin is free, it will require LiteSpeed Enterprise to make it work. Installing LiteMage on OLS won’t work.</li>
</ol>
</li>
</ol>