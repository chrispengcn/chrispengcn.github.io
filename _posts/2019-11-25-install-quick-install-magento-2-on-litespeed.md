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
<ol>
 	<li>
<ol>
 	<li>
<ol>
 	<li><a href="https://openlitespeed.org/kb/magento2/#InstallPHPmodules">Install PHP modules</a></li>
 	<li><a href="https://openlitespeed.org/kb/magento2/#MariaDBInstallation">MariaDB Installation</a></li>
 	<li><a href="https://openlitespeed.org/kb/magento2/#CreateMagentoDBAccount">Create MagentoDB Account</a></li>
 	<li><a href="https://openlitespeed.org/kb/magento2/#CreateMagentouser">Create a Magento User</a></li>
 	<li><a href="https://openlitespeed.org/kb/magento2/#DownloadandExtractMagento">Download and Extract Magento</a></li>
 	<li><a href="https://openlitespeed.org/kb/magento2/#createvirtualhost">Create a Virtual Host for Magento</a></li>
 	<li><a href="https://openlitespeed.org/kb/magento2/#RuntheMagentoinstallscript">Run the Magento install script</a></li>
 	<li><a href="https://openlitespeed.org/kb/magento2/#changedocroot">Good Pratice: Changing doc_root to /pub/</a></li>
</ol>
<h3><span id="Install_PHP_Modules" class="ez-toc-section"><a id="InstallPHPmodules"></a>Install PHP Modules</span></h3>
The easiest way to install PHP for OpenLiteSpeed (without ols1clk) is through our CentOS/Ubuntu/Debian repository. If the LiteSpeed Repository was not installed and enabled during the web server installation, follow <a href="https://www.litespeedtech.com/support/wiki/doku.php?id=litespeed_wiki:php:rpm" target="_blank" rel="noopener noreferrer">this guide</a> to install and enable the LiteSpeed Repository.

Installing PHP for OLS includes installing RPM or APT packages, setting up LSPHP external apps, and setting up script handlers. Please refer to <a href="https://www.litespeedtech.com/support/wiki/doku.php/litespeed_wiki:php:configuring-lsws-for-php">this wiki</a> for details.

Magento 2.3.1 is certified and tested on PHP 7.2.11, so you can run the following command to install LSPHP72:

CentOS:
<pre class="line-numbers language-markup code-toolbar"><code class=" language-markup">yum install lsphp72 lsphp72-mysqlnd lsphp72-common lsphp72-gd lsphp72-pdo lsphp72-process lsphp72-mbstring lsphp72-mcrypt lsphp72-opcache lsphp72-bcmath lsphp72-xml lsphp72-soap lsphp72-json lsphp72-intl -y</code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
Ubuntu/Debian:
<pre class="line-numbers language-markup code-toolbar"><code class=" language-markup">apt-get install lsphp72 lsphp72-mysqlnd lsphp72-common lsphp72-gd lsphp72-pdo lsphp72-process lsphp72-mbstring lsphp72-mcrypt lsphp72-opcache lsphp72-bcmath lsphp72-xml lsphp72-soap lsphp72-json  -intl -y</code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
then set up the internal app pointing to the newly installed LSPHP binary. Lastly set up the script handler.
<h3><span id="MariaDBMySQL_Installation" class="ez-toc-section"><a id="MariaDBInstallation"></a>MariaDB/MySQL Installation</span></h3>
On CentOS, run the following to install MariDB:
<pre class="line-numbers language-bash code-toolbar"><code class=" language-bash">yum <span class="token function">install</span> mariadb-server</code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
On Ubuntu/Debian, you will need to install MySQL instead of MariaDB.
<pre class="line-numbers language-markup code-toolbar"><code class=" language-markup">apt-get install mysql-server</code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
Start MariaDB/MySQL server:
<pre class="line-numbers language-markup code-toolbar"><code class=" language-markup">systemctl start mysql</code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
Set a new password:
<pre class="line-numbers language-bash code-toolbar"><code class=" language-bash">/usr/bin/mysql_secure_installation</code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
Enter <code>$yourpassword</code>
<h3><span id="Create_a_Magento_Database_Account" class="ez-toc-section"><a id="CreateMagentoDBAccount"></a>Create a Magento Database Account</span></h3>
<pre class="line-numbers language-bash code-toolbar"><code class=" language-bash">mysql -u root -p<span class="token string">'yourmysqlpassword'</span></code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
<pre class="line-numbers language-bash code-toolbar"><code class=" language-bash">create database magento<span class="token punctuation">;</span> 
grant all privileges on magento.* to magento@localhost identified by <span class="token string">'magento-database-password'</span><span class="token punctuation">;</span> 
<span class="token keyword">exit</span><span class="token punctuation">;</span></code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
<h3><span id="Create_a_Magento_User" class="ez-toc-section"><a id="CreateMagentouser"></a>Create a Magento User</span></h3>
To better isolate your applications, let’s create a user only for Magento.

Create magento2 user:
<pre class="line-numbers language-markup code-toolbar"><code class=" language-markup">useradd magento2</code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
Change magento2 password:
<pre class="line-numbers language-markup code-toolbar"><code class=" language-markup">passwd magento2</code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
<h3><span id="Download_and_Extract_Magento" class="ez-toc-section"><a id="DownloadandExtractMagento"></a>Download and Extract Magento</span></h3>
Now we can <a href="https://magento.com/tech-resources/download">download Magento 2</a> from their official page and prepare our file structure for the next steps.
<pre class="line-numbers language-markup code-toolbar"><code class=" language-markup">cd /home/magento2/public_html/
### copy Magento-CE-2.2.4_sample_data.zip to here
unzip Magento-*.zip 
### Change files permission
chown -R magento2:  /home/magento2/public_html/</code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
<h3><span id="Create_a_Virtual_Host" class="ez-toc-section"><a id="createvirtualhost"></a>Create a Virtual Host</span></h3>
You need to create a new Virtual Host. Go to OLS Admin Console -&gt; Configuration -&gt;  <b>Virtual Hosts</b> and click on the ‘ + ‘ (<b>Add</b>) sign.

Configure the values according to your situation. When you finish, click on the <b>Save</b> icon. It will complain about the config file not existing. Just click on <b>‘CLICK TO CREATE’</b> and try to save again.

Next, you will be redirected to Virtual Hosts Summary again. Click on the Virtual Host you just created to open its options and continue to do the basic configuration.

At the beginning, you should configure Magento Document Root as: <strong>/home/magento2/public_html/</strong></li>
</ol>
</li>
 	<li></li>
 	<li>     You MUST <b>enable Rewrites</b> and “<b>Auto Load from .htaccess</b>” also. Magento 2 relies on .htaccess to work properly. Assuming your PHP, Script Handler, log and other options are already set, you can now save everything and reload OpenLiteSpeed by clicking on the <b>Graceful Restart</b> button.
<h3><span id="Run_the_Magento_Install_Script" class="ez-toc-section"><a id="RuntheMagentoinstallscript"></a>Run the Magento Install Script</span></h3>
<h4><span id="Step_1_Run_Installation_Script" class="ez-toc-section">Step 1. Run Installation Script</span></h4>
Point your browser with default port to https://yourstore.com/
Accept terms and conditions by clicking ‘Accept and Setup Magento’.

<img class="alignnone size-full wp-image-3343" src="https://www.hss5.com/wp-content/uploads/2019/11/magento2-1-300x253.png" width="300" height="253" alt="" />
<h4><span id="Step_2_Readiness_Check" class="ez-toc-section">Step 2. Readiness Check</span></h4>
<div class="level3">

<img class="alignnone size-full wp-image-3344" src="https://www.hss5.com/wp-content/uploads/2019/11/readiness_check-1024x462.png" width="1024" height="462" alt="" />

</div>
Magento CE installer needs to check if all the requirements are met. If you have followed all the guidelines above, all the requirements should be met, so just click ‘Start Readiness Check’.

<img class="alignnone size-full wp-image-3345" src="https://www.hss5.com/wp-content/uploads/2019/11/magento2-3-1024x567.png" width="1024" height="567" alt="" />

&nbsp;
<h4 id="set_up_database" class="sectionedit11"><span id="Step_3_Set_up_Database" class="ez-toc-section">Step 3. Set up Database</span></h4>
We’ve created the database above. Just enter the database details and click <strong>Save and continue</strong>. If the Magento installer is successfully able to connect to the database, it will start the installation process.

Enter the database details we created above.

<img class="alignnone size-full wp-image-3346" src="https://www.hss5.com/wp-content/uploads/2019/11/magento-database-1024x591.png" width="1024" height="591" alt="" />
<h4 id="configure_site" class="sectionedit12"><span id="Step_4_Web_Configuration" class="ez-toc-section">Step 4. Web Configuration</span></h4>
<div class="level3"></div>
Set up your Store and Admin URL Path, e.g.
<ul>
 	<li class="level1">
<div class="li">Your Store Address: <a class="urlextern" title="http://yourdomain.com" href="http://yourdomain.com/" rel="nofollow">http://yourdomain.com/</a></div></li>
 	<li class="level1">
<div class="li">Magento Admin Address: <a class="urlextern" title="http://yourdomain.com/admin" href="http://yourdomain.com/admin" rel="nofollow">http://yourdomain.com/admin</a></div></li>
</ul>
<img class="alignnone size-full wp-image-3347" src="https://www.hss5.com/wp-content/uploads/2019/11/magento-web-configurations-1024x544.png" width="1024" height="544" alt="" />
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
<img class="alignnone size-full wp-image-3348" src="https://www.hss5.com/wp-content/uploads/2019/11/magento2-5-1024x324.png" width="1024" height="324" alt="" />
<h3></h3>
<h3><span id="Good_Practice_Change_document_root" class="ez-toc-section"><a id="changedocroot"></a>Good Practice: Change document root</span></h3>
</li>
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