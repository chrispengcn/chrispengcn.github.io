---
ID: 3804
post_title: Wordpress generate static website
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://vpseo.com/2020/11/23/wordpress-generate-static-website/
published: true
post_date: 2020-11-23 16:20:29
---
<a href="https://wp2static.com/developers/wp-cli/">https://wp2static.com/developers/wp-cli/</a>

&nbsp;
<h1>WP-CLI</h1>
&nbsp;

<a href="https://wp-cli.org/">WP-CLI</a> is a developer-friendly way to manage WordPress sites. WP2Static integrates with WP-CLI, adding useful commands to generate and deploy your static site.
<h2 id="wp-cli-commands">WP-CLI commands</h2>
Running the command <code>wp wp2static</code> will give you all the avaiable options and is the best way to know which commands are available on your system. WP2Static Add-ons may provide additional commands.

WP-CLI commands will apply to the site relative to the system path you run them from.
<h3 id="system-user-when-running-wp-cli">System user when running WP-CLI</h3>
TL;DR - run <code>wp</code> commands as the same user as your webserver

When WP-CLI runs, it will likely be using the system default PHP, which executes in a different environment than when accessing your WP site via the webserver. This means that the PHP configuration is likely pulled from a <code>php.ini</code> vs a <code>php-fpm.conf</code> type file, so check you have any webserver environment customizations you need also in your CLI PHP environment.

It also means that when you run the <code>wp</code> command from the CLI, you’ll be doing so as the currently logged in user, unless specified otherwise. For WP2Static and other plugins which touch files on the webserver, this can lead to issues with file permissions, ie - <code>mylocaluser</code> runs <code>wp wp2static generate</code>, which sets files with ownership of <code>mylocaluser</code>, but then when you perform an action via the webserver, your webserver user (<code>www</code>, <code>www-data</code>, etc) may not have permissions to use those files, resulting in unexpected errors.

To change the user who runs <code>wp</code> commands:
<h4 id="openbsd">OpenBSD</h4>
Edit your <code>/etc/doas.conf</code> file, adding a line:

<code>permit persist MYUSERNAME as www cmd wp</code>

You may then choose to alias the command for easier usage. In your <code>~/.profile</code>:

<code>alias wp='doas -u www wp'</code>

Alternatively, configure your web root to be owned by <code>{MY_USER}:www</code> and be group-writable, then you may run <code>wp</code> as your regular user.
<h4 id="linuxmacoswindows">Linux/macOS/Windows</h4>
Try something similar with <code>sudo -u www wp</code> or <code>runas /noprofile /user:mymachine\www wp</code>. <a href="https://wp2static.com/contact">Let us know</a> what works and we’ll update this doc.
<h3 id="multisite-wp2static-wp-cli-commands">Multisite WP2Static WP-CLI commands</h3>
When running WordPress in a Network/Multisite setup, add the <code>--url=&lt;url&gt;</code> argument to force the command to be applied to the WordPress site with that Site URL.
<h3 id="undefined-environment-variables-when-running-wp-cli-commands">Undefined environment variables when running WP-CLI commands</h3>
<ul>
 	<li><code>$_SERVER['SERVER_NAME']</code></li>
 	<li><code>$_SERVER['HTTP_HOST']</code></li>
 	<li><code>$_SERVER['DOCUMENT_ROOT']</code></li>
</ul>
You may need to override these in your <code>/wp-config.php</code> file to get identical behaviour when executing WP2Static commands via WP-CLI vs from within the WordPress web application.
<h3 id="wp-cli-examples">WP-CLI examples</h3>

<hr />

<h4 id="quickstart-install-and-activate-wp2static-set-deployment-url-and-generate-static-site">Quickstart: Install and activate WP2Static, set deployment URL and generate static site</h4>
<ul>
 	<li><code>wp plugin install --activate static-html-output-plugin</code></li>
 	<li><code>wp wp2static options set deploymentURL https://example.com</code></li>
 	<li><code>wp wp2static detect</code></li>
 	<li><code>wp wp2static crawl</code></li>
 	<li><code>wp wp2static post_process</code></li>
</ul>
Your static site will now be available at wp-content/uploads/wp2static-processed-site.

Note: the plugin’s <code>slug</code>, as used within the official wp.org repo is <code>static-html-output-plugin</code>, not <code>wp2static</code>. Because, <a href="https://wp2static.com/about/">reasons</a>.

<hr />

<h4 id="quickstart-generate-static-site-and-deploy-using-netlify-cli-tool">Quickstart: Generate static site and deploy using Netlify CLI tool</h4>
This is a good option for very large sites deploying to Netlify, as only changes will be deployed each time.
<ul>
 	<li><code>wp wp2static options set deploymentURL https://example.com</code></li>
 	<li><code>wp wp2static detect</code></li>
 	<li><code>wp wp2static crawl</code></li>
 	<li><code>wp wp2static post_process</code></li>
 	<li><code>cd wp-content/uploads/wp2static-processed-site</code></li>
 	<li><code>netlify deploy</code></li>
</ul>
The <a href="https://github.com/netlify/cli">Netlify CLI</a> will give you an interactive prompt to follow.

<hr />

<h4 id="quickstart-generate-static-site-and-deploy-via-sftp">Quickstart: Generate static site and deploy via sFTP</h4>
Until there is a dedicated Add-on supporting sFTP, the following commands will work.
<ul>
 	<li><code>wp wp2static options set deploymentURL https://example.com</code></li>
 	<li><code>wp wp2static detect</code></li>
 	<li><code>wp wp2static crawl</code></li>
 	<li><code>wp wp2static post_process</code></li>
 	<li><code>scp -rp wp-content/uploads/wp2static-processed-site user@server:dest</code></li>
</ul>

<hr />

<h4 id="quickstart-generate-static-site-and-deploy-to-cloudflare-workers-via-wrangler">Quickstart: Generate static site and deploy to CloudFlare Workers via Wrangler</h4>
Until there is a dedicated Add-on supporting CloudFlare workers, the following commands will work.
<ul>
 	<li><code>wp wp2static options set deploymentURL https://wp2staticv6test.wp2static.workers.dev/</code></li>
 	<li><code>wp wp2static detect</code></li>
 	<li><code>wp wp2static crawl</code></li>
 	<li><code>wp wp2static post_process</code></li>
 	<li><code>cp -r wp-content/uploads/wp2static-processed-site/* "$MY_WORKER_DIR"/public/</code></li>
 	<li><code>cd "$MY_WORKER_DIR"</code></li>
 	<li><code>wrangler publish</code></li>
</ul>
Above example expects you’ve installed CloudFlare’s <a href="https://github.com/cloudflare/wrangler">Wrangler</a> tool and run <code>wrangler config</code> and <code>wrangler generate --site SITE_NAME</code>. <code>MY_WORKER_DIR</code> above is to be replaced with the actual directory you created with <code>wrangler generate</code>.

Your <code>wrangler.toml</code> will look similar to this:
<pre><code>name = "wp2staticv6test"
type = "webpack"
account_id = "0a5e6f4c0d8a4e4cb4dd08d29e99ddcc"
workers_dev = true

[site]
bucket = "./public"
</code></pre>
This example shows how to deploy to your <code>workers.dev</code> domain. Additional configuration is required to deploy to your own domain name. To be updated.
<h3 id="wp2static-core-cli-commands-explained">WP2Static Core CLI commands explained</h3>
<h3 id="detect">detect</h3>
Detects URLs within WordPress site to be used in crawling. Detection levels controlled by core options and any optional detection add-ons.
<ul>
 	<li><code>wp wp2static detect</code></li>
</ul>
Detected URLs go into WP2Static’s Crawl Queue.
<ul>
 	<li><code>wp wp2static crawl_queue count # get total number of detected URLs</code></li>
 	<li><code>wp wp2static crawl_queue list # list all detected URLs</code></li>
 	<li><code>wp wp2static crawl_queue delete # clear Crawl Queue</code></li>
</ul>
<h3 id="crawl">crawl</h3>
Crawls all URLs in the Crawl Queue, generating a static site, identical to your WordPress site, but with URLs like <code>/some-post/</code> generating static site compatible folder called <code>/some-post/</code>, containing an <code>index.html</code> file within.
<ul>
 	<li><code>wp wp2static crawl</code></li>
</ul>
Generates static site to <code>wp-content/uploads/wp2static-crawled-site</code>.

Crawl Cache is used to prevent having to re-crawl URLs which haven’t changed.
<ul>
 	<li><code>wp wp2static crawl_cache count # get total number of cached crawl URLs</code></li>
 	<li><code>wp wp2static crawl_cache list # list cached crawl URLs</code></li>
 	<li><code>wp wp2static crawl_cache delete # rm wp-content/uploads/wp2static-crawled-site</code></li>
</ul>
<h3 id="post_process">post_process</h3>
Processes the crawled site, rewriting URLs and optionally performing other actions to prepare static site for deployment.
<ul>
 	<li><code>wp wp2static post_process</code></li>
</ul>
Generates deployable static site files to <code>wp-content/uploads/wp2static-processed-site</code>.
<h3 id="deploy">deploy</h3>
Triggers a deployment, which signals to any deployment add-ons to perform their specific deployment actions.
<ul>
 	<li><code>wp wp2static deploy</code></li>
</ul>
Triggers post-deployment actions on completion, such as email or webhook notifications.
<h3 id="process_queue">process_queue</h3>
This will check for any queued jobs with the ‘waiting’ status and process them.
<ul>
 	<li><code>wp wp2static process_queue</code></li>
</ul>
This is useful when you want to rely on regular CRON job than WP-Cron.

&nbsp;