---
ID: 1068
post_title: >
  How to Setup Magento with Multiple
  Stores and Domains
author: chrispengcn
post_excerpt: |
  One of the useful features in Magento is the ability to create multiple stores/websites that share the same Magento installation. This allows multiple store fronts to share a common code base and backend, making administration a lot easier. Stores can share customer base, product catalog and settings based on how you choose to configure your sites.
  
  This tutorial will go through the steps of setting up multiple stores in Magento, and how to configure a domain for each store.
  https://www.properhost.com/support/kb/30/How-To-Setup-Magento-With-Multiple-Stores-And-Domains
layout: post
permalink: >
  http://hss5.com/2017/10/09/how-to-setup-magento-with-multiple-stores-and-domains/
published: true
post_date: 2017-10-09 11:29:49
---
One of the useful features in Magento is the ability to create multiple stores/websites that share the same Magento installation. This allows multiple store fronts to share a common code base and backend, making administration a lot easier. Stores can share customer base, product catalog and settings based on how you choose to configure your sites.

This tutorial will go through the steps of setting up multiple stores in Magento, and how to configure a domain for each store.
<h2>Prerequisites</h2>
<ul>
 	<li>Magento must be installed</li>
 	<li>Access to add new vhosts (virtual hosts). Most control panels (like cPanel) supports this.</li>
</ul>
<h2>URL structure</h2>
Before you start adding new stores to Magento, you should have a clear understanding of how each store will be addressed, i.e. how the URL will look like. You basically have three approaches:
<ul>
 	<li>Domain (e.g. www.store1.com and www.store2.com)</li>
 	<li>Subdomain (e.g. store1.mystore.com and store2.mystore.com)</li>
 	<li>Folder (e.g. mystore.com/store1/ and mystore.com/store2/)</li>
</ul>
How you choose to structure your stores is a matter of personal preference, and the steps for setting this up will be more or less the same. Generally, subdomains are often used to create localized or area-specific versions of the store (e.g. es.mybestshop.com and en.mybestshop.com). Since most users will choose either the domain or subdomain method, this is the method I will describe in this tutorial.
<h2>Server configuration</h2>
Before we create the new Magento stores, we need to configure each domain/subdomain to resolve to the same Magento installation. That is, the document root of each <strong><em>virtual host </em></strong>must be set to the file path where Magento is installed. How this is done depends on the web server and control panel. For this example, we are assuming the server is running the Apache Web Server and the cPanel control panel. The method will be relatively similar for other control panels.

Assuming your main domain is already set up in cPanel, we just need to add the additional domains/subdomains. We will use the <strong>Parked Domains</strong> feature in cPanel to add the additional domains. Parked domains are similar to aliases, so they will automatically resolve to the same file directory. If you chose the subdomain method, just create the subdomains and point them to the directory where Magento is installed.

<a href="https://www.properhost.com/img/kb/cpanel-parked-domain1.jpg" target="_blank" rel="noopener noreferrer"><img src="https://www.properhost.com/img/kb/cpanel-parked-domain1-300x242.jpg" alt="Parked Domains" width="300" height="242" /></a> <a href="https://www.properhost.com/img/kb/cpanel-parked-domain2.jpg" target="_blank" rel="noopener noreferrer"><img src="https://www.properhost.com/img/kb/cpanel-parked-domain2-300x121.jpg" alt="Parked Domain in cPanel" width="300" height="121" /></a>
<h2>Add new Magento store</h2>
The next step is to create the actual stores in Magento. Lets say we want to add a new store <strong>mysecondstore.com</strong>.
<h3>Create root category</h3>
If you want your websites to share the same catalog and categories you can skip this step.
<ol>
 	<li>Log into your Magento Admin Panel</li>
 	<li>Click on <strong>Catalog </strong>&gt; <strong>Manage Categories
<a href="https://www.properhost.com/img/kb/magento-categories.jpg"><img src="https://www.properhost.com/img/kb/magento-categories.jpg" alt="magento-categories" width="220" height="267" /></a></strong></li>
 	<li>Click on <strong>Add Root Category
</strong><a href="https://www.properhost.com/img/kb/magento-root-catalog.jpg"><img class="alignnone size-full wp-image-64" src="https://www.properhost.com/img/kb/magento-root-catalog.jpg" alt="magento-root-catalog" width="658" height="384" /></a></li>
 	<li>Enter a name for the new category and make sure <strong>Is Active</strong><strong> </strong>is set to <strong>True</strong></li>
 	<li>Click on the <strong>Display Settings </strong>tab and set <strong>Is Anchor</strong> to <strong>True.</strong> This will show products listed in sub categories, and also enable product drill-down functionality (filtering) for the category.<strong>
<a href="https://www.properhost.com/img/kb/magento-category-anchor1.jpg"><img src="https://www.properhost.com/img/kb/magento-category-anchor1.jpg" alt="magento-category-anchor" width="536" height="176" /></a></strong></li>
 	<li>Click <strong>Save Category</strong></li>
</ol>
<h3>Store configuration</h3>
Before we start configuring our stores, lets pause for a minute and explain the concepts of <strong>Websites</strong>,<strong> Stores</strong>, and <strong>Store Views </strong>in Magento. Websites are the top-most entity in Magento. If you want completely separate sites that do not share cart, shipping methods, etc., you should create separate <strong>Websites</strong>. Each Website has at least one <strong>Store</strong>, and each Store has at least one <strong>Store View</strong>. Multiple <strong>Stores </strong>can share cart, user sessions, payment gateways, etc., but have their own catalog structure. Finally, a <strong>Store </strong>is a collection of <strong>Store Views. Store Views</strong> change the way pages are presented, normally used to offer a site in different layouts or languages. These concepts can be a bit confusing at first and I recommend <a href="http://www.magentocommerce.com/blog/comments/getting-the-most-of-multi-store-retailing-webinar-transcript/">this webinar</a> which explains the Magento multi-store retailing in detail.
<ol>
 	<li>Go to <strong>System &gt; Manage Stores</strong></li>
 	<li>Click <strong>Create Website </strong>and enter the following information:<strong>
</strong><em>(Skip this step if you don't want separate <strong>Websites</strong>)</em>
<ul>
 	<li><strong>Name</strong> - enter a name for the new website</li>
 	<li><strong>Code</strong> - enter a unique identifier for this website
<em id="__mceDel"><strong>Make a note of this code as you will need it later!</strong></em></li>
</ul>
<a href="https://www.properhost.com/img/kb/magento-create-website.jpg"><img src="https://www.properhost.com/img/kb/magento-create-website.jpg" alt="magento-create-website" width="570" height="228" /></a></li>
 	<li>Click <strong>Create Store</strong> and enter the following:
<ul>
 	<li><strong>Website </strong><strong>- </strong>select the website you just created from the dropdown;</li>
 	<li><strong>Name</strong> - enter a name for the store;</li>
 	<li><strong>Root Category</strong> - select the category<strong> </strong>you created in the previous step.</li>
</ul>
<a href="https://www.properhost.com/img/kb/magento-add-store.jpg"><img src="https://www.properhost.com/img/kb/magento-add-store.jpg" alt="magento-add-store" width="574" height="240" /></a></li>
 	<li>Click <strong>Create Store View</strong> and the information as follows
<ul>
 	<li><strong>Store</strong> - select the store you created in the previous step</li>
 	<li><strong>Name </strong>- enter a name for the store view</li>
 	<li><strong>Code</strong> - enter a unique identifier for this store view
<em><strong>Make a note of this code as you will need it later!</strong></em></li>
</ul>
This is where you will add additional store views if you plan on setting up a site for each language you support for example.
<a href="https://www.properhost.com/img/kb/magento-add-storeview.jpg"><img src="https://www.properhost.com/img/kb/magento-add-storeview.jpg" alt="magento-add-storeview" width="539" height="246" /></a></li>
 	<li>Now, go to <strong>System &gt; Configuration &gt; General</strong></li>
 	<li>Make sure <strong>Default Config </strong>is selected as the <strong>Current Configuration Scope
<a href="https://www.properhost.com/img/kb/magento-config-scope.jpg"><img src="https://www.properhost.com/img/kb/magento-config-scope.jpg" alt="magento-config-scope" width="254" height="214" /></a>
</strong></li>
 	<li>On the <strong>Web </strong>tab, set <strong>Auto-redirect to Base URL</strong> to <strong>No</strong> and click <strong>Save Config
<a href="https://www.properhost.com/img/kb/magento-web-settings.jpg"><img src="https://www.properhost.com/img/kb/magento-web-settings.jpg" alt="magento-web-settings" width="300" height="88" /></a></strong></li>
 	<li>Now, change the <strong>Configuration Scope</strong> dropdown to you newly created website</li>
 	<li>Under the <strong>Web</strong> section, we now need to change the <strong>Secure Base URL </strong>and <strong>Unsecure Base URL </strong>settings. Uncheck the <strong>Use Default [STORE VIEW]</strong>, and replace the URL's with your corresponding domain name. <em>Remember to include the trailing /.
<a href="https://www.properhost.com/img/kb/magento-site-urls.jpg"><img src="https://www.properhost.com/img/kb/magento-site-urls.jpg" alt="magento-site-urls" width="571" height="486" /></a></em></li>
 	<li>Click <strong>Save Configuration</strong></li>
</ol>
This completes the configuration of the new store. Repeat these steps for each additional store you want to add.
<h2>Domain mapping</h2>
So far we have added the additional domains to the server and configured the new store in Magento. Now we just need to glue it together by telling Magento which store to load based on the domain name the user is on.
<ol>
 	<li>On the server, open the<strong> .htaccess file</strong>, located in the root directory of your Magento installation, in your favorite text editor. You can also use the File Manager in your control panel.</li>
 	<li>Add the following lines (replace with actual domain names):
<pre><code>
SetEnvIf Host www\.domain1\.com MAGE_RUN_CODE=domain1_com
SetEnvIf Host www\.domain1\.com MAGE_RUN_TYPE=website
SetEnvIf Host ^domain1\.com MAGE_RUN_CODE=domain1_com
SetEnvIf Host ^domain1\.com MAGE_RUN_TYPE=website

SetEnvIf Host www\.domain2\.com MAGE_RUN_CODE=domain2_com
SetEnvIf Host www\.domain2\.com MAGE_RUN_TYPE=website
SetEnvIf Host ^domain2\.com MAGE_RUN_CODE=domain2_com
SetEnvIf Host ^domain2\.com MAGE_RUN_TYPE=website

...</code></pre>
Note: the <strong>SetEnvIf</strong> directive is not supported by all web servers (e.g. LiteSpeed). In that case, the store code can be set in this way:
<pre><code>
RewriteCond %{HTTP_HOST} www\.domain1\.com [NC]
RewriteRule .* - [E=MAGE_RUN_CODE:domain1_com]
RewriteCond %{HTTP_HOST} www\.domain1\.com [NC]
RewriteRule .* - [E=MAGE_RUN_TYPE:website]

RewriteCond %{HTTP_HOST} www\.domain2\.com [NC]
RewriteRule .* - [E=MAGE_RUN_CODE:domain2_com]
RewriteCond %{HTTP_HOST} www\.domain2\.com [NC]
RewriteRule .* - [E=MAGE_RUN_TYPE:website]

</code></pre>
<ul>
 	<li><strong>MAGE_RUN_CODE</strong> - this is the unique code chosen when you created the Magento <strong>Website</strong> / <strong>Store View</strong></li>
 	<li><strong>MAGE_RUN_TYPE</strong> - depending on whether you want to load a specific <strong>Website </strong>or <strong>Store View;</strong> set it to <strong>website </strong>or <strong>store</strong>, respectively.</li>
</ul>
Add an entry for each additional domain you have set up.</li>
 	<li>Save the file</li>
</ol>
Now navigate to you new site and verify that the correct Magento store is loaded. If not, make sure that the the <strong>MAGE_RUN_CODE</strong> match the one created earlier and that the domain resolves to the correct folder/path.
<h4><strong>Note about Magento versions prior to 1.4</strong></h4>
The ability to set store code using environmental variables were introduced in Magento 1.4. If you are running an older version of Magento, or your webserver does not support Apache environmental variables, you must set the store code using a different method.
<ol>
 	<li>Open <strong>index.php</strong> in any text editor</li>
 	<li>Look for the following line near the bottom of the file:
<pre><code>Mage::run($mageRunCode, $mageRunType);</code></pre>
</li>
 	<li>Add the following lines right above this line:
<pre><code>switch($_SERVER['HTTP_HOST']) {
    case 'domain1.com':
    case 'www.domain1.com':
        $mageRunCode = 'domain1_com';
        $mageRunType = 'website';
    break;
    case 'domain2.com':
    case 'www.domain2.com':
        $mageRunCode = 'domain2_com';
        $mageRunType = 'website';
    break;
}</code></pre>
Replace the <strong>domain</strong>, <strong>code</strong> and <strong>type</strong> according to your particular setup. Add additional <em>case statements</em> if you have more stores.</li>
 	<li>Save the file</li>
</ol>
You should now be able to see each respective store by browsing the URL.

Good luck!

&nbsp;

https://www.properhost.com/support/kb/30/How-To-Setup-Magento-With-Multiple-Stores-And-Domains