---
ID: 1263
post_title: >
  Tutorial—Set up multiple websites or
  stores with nginx
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/07/05/tutorial-set-up-multiple-websites-or-stores-with-nginx/
published: true
post_date: 2018-07-05 19:17:28
---
<section class="page-intro">
<h1 class="page-heading">Tutorial—Set up multiple websites or stores with nginx</h1>
</section>
<h2 id="ms-nginx-over">Set up multiple websites with nginx</h2>
&nbsp;

<a href="https://devdocs.magento.com/guides/v2.0/config-guide/multi-site/ms_nginx.html">https://devdocs.magento.com/guides/v2.0/config-guide/multi-site/ms_nginx.html</a>

&nbsp;

This tutorial shows you step-by-step how to set up multiple websites using <span class="glossary-term" data-toggle="popover">nginx</span> .
<h3 id="assumptions">Assumptions</h3>
We assume the following:
<ul>
 	<li>You’re working on a development machine (laptop, virtual machine, and so on)Additional tasks might be required to deploy multiple websites in a hosted environment; check with your hosting provider for more information.

Additional tasks are required to set up Magento Commerce (Cloud). After you complete the tasks discussed in this topic, see <a href="https://devdocs.magento.com/guides/v2.0/cloud/project/project-multi-sites.html">Set up multiple Magento Commerce (Cloud) websites or stores</a>.</li>
 	<li>You use one virtual host per website; the virtual host configuration files are located in <code class="highlighter-rouge">/etc/nginx/sites-available</code></li>
 	<li>You use <code class="highlighter-rouge">nginx.conf.sample</code> provided by Magento with only the modifications discussed in this tutorial, and two line configurations.</li>
 	<li>The Magento software is installed in <code class="highlighter-rouge">/var/www/html/magento2</code></li>
 	<li>You have two websites other than the default:
<ul>
 	<li><code class="highlighter-rouge">french.mysite.mg</code> with website code <code class="highlighter-rouge">french</code> and store view code <code class="highlighter-rouge">fr</code></li>
 	<li><code class="highlighter-rouge">german.mysite.mg</code> with website code <code class="highlighter-rouge">german</code> and store view code <code class="highlighter-rouge">de</code></li>
</ul>
<div class="bs-callout bs-callout-tip">

Refer to <a href="https://devdocs.magento.com/guides/v2.0/config-guide/multi-site/ms_websites.html#step-2-create-websites">Create websites</a> and <a href="https://devdocs.magento.com/guides/v2.0/config-guide/multi-site/ms_websites.html#step-4-create-store-views">Create store views</a> for help locating these values.

</div></li>
</ul>
<h3 id="roadmap-for-setting-up-multiple-websites-with-nginx">Roadmap for setting up multiple websites with nginx</h3>
Setting up multiple stores consists of the following tasks:
<ol>
 	<li><a href="https://devdocs.magento.com/guides/v2.0/config-guide/multi-site/ms_websites.html">Set up websites, stores, and store views</a> in the <span class="glossary-term" data-toggle="popover">Magento Admin</span> .</li>
 	<li>Create one <a href="https://devdocs.magento.com/guides/v2.0/config-guide/multi-site/ms_nginx.html#ms-nginx-vhosts">nginx virtual host</a> per Magento <span class="glossary-term" data-toggle="popover">website</span> .</li>
 	<li>Pass the values of the <a href="https://devdocs.magento.com/guides/v2.0/config-guide/multi-site/ms_over.html">Magento variables</a> <code class="highlighter-rouge">$MAGE_RUN_TYPE</code> and <code class="highlighter-rouge">$MAGE_RUN_CODE</code> to nginx using the Magento-provided <code class="highlighter-rouge">nginx.conf.sample</code>.
<ul>
 	<li><code class="highlighter-rouge">$MAGE_RUN_TYPE</code> can be either <code class="highlighter-rouge">store</code> or <code class="highlighter-rouge">website</code>
<ul>
 	<li>Use <code class="highlighter-rouge">website</code> to load your website in your storefront.</li>
 	<li>Use <code class="highlighter-rouge">store</code> to load any store view in your storefront.</li>
</ul>
</li>
 	<li><code class="highlighter-rouge">$MAGE_RUN_CODE</code> is the unique website or store view code that corresponds to <code class="highlighter-rouge">$MAGE_RUN_TYPE</code></li>
</ul>
</li>
</ol>
<h2 id="ms-nginx-vhosts">Step 2: Create nginx virtual hosts</h2>
This section discusses how to load websites on the <span class="glossary-term" data-toggle="popover">storefront</span> . You can use either websites or store views; if you use store views, you must adjust parameter values accordingly. You must complete the tasks in this section as a user with <code class="highlighter-rouge">root</code> privileges.
<div class="collapsible active">

<b class="collapsible-title">To create virtual hosts: </b>
<div class="collapsible-content">
<ol>
 	<li>Open a text editor and add the following contents to a new file named <code class="highlighter-rouge">/etc/nginx/sites-available/french.mysite.mg.conf</code>:
<div class="highlighter-rouge">
<div class="highlight">
<div class="pre-wrap">
<pre class="highlight"><code>map $http_host $MAGE_RUN_CODE {
   french.mysite.mg french;
}
map $http_host $MAGE_RUN_TYPE {
    french.mysite.mg store
}

server {
   listen 80;
   server_name french.mysite.mg;
   set $MAGE_ROOT /var/www/html/magento2;
   set $MAGE_MODE developer;
   include /var/www/html/magento2/nginx.conf;
}
</code></pre>
</div>
</div>
</div></li>
</ol>
</div>
</div>
Create another file named <code class="highlighter-rouge">german.mysite.mg.conf</code> in the same directory with the following contents:
<div class="highlighter-rouge">
<div class="highlight">
<div class="pre-wrap">
<pre class="highlight"><code>map $http_host $MAGE_RUN_CODE {
   german.mysite.mg german;
}
map $http_host $MAGE_RUN_TYPE {
    german.mysite.mg store
}

server {
   listen 80;
   server_name german.mysite.mg;
   set $MAGE_ROOT /var/www/html/magento2;
   set $MAGE_MODE developer;
   include /var/www/html/magento2/nginx.conf;
}
</code></pre>
</div>
</div>
</div>
&nbsp;
<ul>
 	<li>Save your changes to the files and exit the text editor.</li>
 	<li>Change <code class="highlighter-rouge">nginx.conf.sample</code> content
<div class="highlighter-rouge">
<div class="highlight">
<div class="pre-wrap">
<pre class="highlight"><code>location ~ (index|get|static|report|404|503|health_check)\.php$ {
    try_files $uri =404;
    fastcgi_pass   fastcgi_backend;
    fastcgi_buffers 1024 4k;

    fastcgi_param  PHP_FLAG  "session.auto_start=off \n suhosin.session.cryptua=off";
    fastcgi_param  PHP_VALUE "memory_limit=756M \n max_execution_time=18000";
    fastcgi_read_timeout 600s;
    fastcgi_connect_timeout 600s;

    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
</code></pre>
</div>
</div>
</div></li>
</ul>
&nbsp;

to
<div class="highlighter-rouge">
<div class="highlight">
<div class="pre-wrap">
<pre class="highlight"><code>location ~ (index|get|static|report|404|503|health_check)\.php$ {
    try_files $uri =404;
    fastcgi_pass   fastcgi_backend;
    fastcgi_buffers 1024 4k;

    fastcgi_param  PHP_FLAG  "session.auto_start=off \n suhosin.session.cryptua=off";
    fastcgi_param  PHP_VALUE "memory_limit=756M \n max_execution_time=18000";
    fastcgi_param  MAGE_RUN_TYPE $MAGE_RUN_TYPE;
	    fastcgi_param  MAGE_RUN_CODE $MAGE_RUN_CODE;
    fastcgi_read_timeout 600s;
    fastcgi_connect_timeout 600s;

    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
</code></pre>
</div>
</div>
</div>
Verify the server configuration:
<div class="highlighter-rouge">
<div class="highlight">
<div class="pre-wrap">
<pre class="highlight"><code>nginx -t
</code></pre>
</div>
</div>
</div>
If successful, the following message displays:
<div class="highlighter-rouge">
<div class="highlight">
<div class="pre-wrap">
<pre class="highlight"><code>nginx: configuration file /etc/nginx/nginx.conf test is successful
</code></pre>
</div>
</div>
</div>
&nbsp;
<ul>
 	<li>If errors display, check the syntax of your virtual host configuration files.</li>
 	<li>Create symbolic links in the <code class="highlighter-rouge">/etc/nginx/sites-enabled</code> directory:
<div class="highlighter-rouge">
<div class="highlight">
<div class="pre-wrap">
<pre class="highlight"><code>cd /etc/nginx/sites-enabled
ln -s /etc/nginx/sites-available/french.mysite.mg french.mysite.mg
ln -s /etc/nginx/sites-available/german.mysite.mg german.mysite.mg
</code></pre>
</div>
</div>
</div></li>
</ul>
&nbsp;
<div class="collapsible active">
<div class="collapsible-content">

For more detail about the map directive, see <a href="http://nginx.org/en/docs/http/ngx_http_map_module.html#map" target="_blank" rel="noopener noreferrer">nginx documentation on the map directive</a>.

</div>
</div>
<h2 id="ms-nginx-verify">Verify your site</h2>
<div>

Unless you have DNS set up for your stores’ URLs, you must add a static route to the host in your <code class="highlighter-rouge">hosts</code> file:
<ol>
 	<li>Locate your operating system’s <a href="https://en.wikipedia.org/wiki/Hosts_(file)#Location_in_the_file_system" target="_blank" rel="noopener noreferrer"><code class="highlighter-rouge">hosts</code> file</a>.</li>
 	<li>Add the static route in the format:
<div class="highlighter-rouge">
<div class="highlight">
<div class="pre-wrap">
<pre class="highlight"><code>&lt;ip address&gt; french.mysite.mg
&lt;ip address&gt; german.mysite.mg
</code></pre>
</div>
</div>
</div></li>
</ol>
</div>
Go to one of the following URLs in your browser:
<div class="highlighter-rouge">
<div class="highlight">
<div class="pre-wrap">
<pre class="highlight"><code>http://mysite.mg/admin
http://french.mysite.mg/frenchstoreview
http://german.mysite.mg/germanstoreview
</code></pre>
</div>
</div>
</div>
You’re done!
<div id="info" class="bs-callout bs-callout-info">
<ul>
 	<li>Additional tasks might be required to deploy multiple websites in a hosted environment; check with your hosting provider for more information.</li>
 	<li>Additional tasks are required to set up Magento Commerce (Cloud); for more information, see <a href="https://devdocs.magento.com/guides/v2.0/cloud/project/project-multi-sites.html">Set up multiple Cloud websites or stores</a></li>
</ul>
</div>
<h3 id="troubleshooting">Troubleshooting</h3>
<ul>
 	<li>If your French and German sites return 404s but your Admin loads, make sure you completed <a href="https://devdocs.magento.com/guides/v2.0/config-guide/multi-site/ms_websites.html#multi-storecode-baseurl">Step 6: Add the store code to the base URL</a>.</li>
 	<li>If all URLs return 404s, make sure you restarted your web server.</li>
 	<li>If the Magento Admin doesn’t function properly, make sure you set up your virtual hosts properly.</li>
</ul>