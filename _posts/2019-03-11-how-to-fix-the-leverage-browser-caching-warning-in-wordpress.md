---
ID: 2570
post_title: >
  How to Fix the Leverage Browser Caching
  Warning in WordPress
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/03/11/how-to-fix-the-leverage-browser-caching-warning-in-wordpress/
published: true
post_date: 2019-03-11 13:45:08
---
<div class="post-6137 post type-post status-publish format-standard has-post-thumbnail hentry category-wordpress-performance tag-webperf">
<div id="post-content-6137" class="user-content mt--40 post-content">

If you have ever run your WordPress website through <a href="https://developers.google.com/speed/pagespeed/insights/" target="_blank" rel="noopener noreferrer">Google PageSpeed Insights</a> or Pingdom then you have probably seen that big yellow <strong>Leverage Browser Caching</strong>warning. And that is probably why you ended up at this post. Today we will dive into what this warning means, how it affects you, and what your options are as it pertains to your WordPress site.
<ul>
 	<li><a href="https://kinsta.com/blog/leverage-browser-caching/#what-is-leverage-browser-caching">What is the Leverage Browser Caching Warning?</a></li>
 	<li><a href="https://kinsta.com/blog/leverage-browser-caching/#fix-leverage-browser-caching-wordpress">Fix the Leverage Browser Caching Warning in WordPress</a></li>
</ul>
<h2 id="what-is-leverage-browser-caching">What is the Leverage Browser Caching Warning?</h2>
The <a href="https://developers.google.com/speed/docs/insights/LeverageBrowserCaching" target="_blank" rel="noopener noreferrer">leverage browser caching</a> warning, as shown below in the screenshot, is referring to your browser cache. Whenever you visit a website, it downloads assets, such as HTML, CSS, JavaScript and images into your browser’s local cache. This way it doesn’t have to retrieve them on every page load. The warning itself is returned when your web server, or a third-party server, doesn’t have the correct HTTP cache headers implemented. Or the headers might exist, but the cache time is set too short.
<div id="attachment_6151" class="wp-caption alignnone"><img class="alignnone size-full wp-image-2574" src="http://hss5.com/wp-content/uploads/2019/03/leverage-browser-caching-pagespeed-insights.png" width="1265" height="235" alt="leverage browser caching pagespeed insights" />
<p class="wp-caption-text">Set an expiry date or age in HTTP headers</p>

</div>
You might also see this warning in the new “<a href="https://testmysite.thinkwithgoogle.com/" target="_blank" rel="noopener noreferrer">think with Google</a>” website speed test, which is powered by Google PageSpeed Insights. This was designed to be more of a marketing tool by Google, but this has resulted in lot of clients now simply forwarding these errors to their webmasters. Leaving WordPress developers and designers looking for ways to quickly fix it to appease their clients.
<div class="cta-mini">
<div class="cta-mini--default">
<h4 class="heading--small mt--0 mb--30 text--superbold">Still looking for that perfect WordPress host?</h4>
<div class="text--normal">
<div class="mb--30">Try Kinsta's premium managed WordPress hosting to experience your site without problems.</div>
<ul class="ml--0 mt--0">
 	<li class="row nomargin nocol middle-xs"><img class="mr--6" src="https://kinsta.com/wp-content/themes/kinsta/images/home-managed.svg" alt="Styleized controls representing management" width="20" />Fully managed</li>
 	<li class="row nomargin nocol middle-xs"><img class="mr--6" src="https://kinsta.com/wp-content/themes/kinsta/images/home-security.svg" alt="Shield with a tick representing security" width="20" />Secure like Fort Knox</li>
 	<li class="row nomargin nocol middle-xs"><img class="mr--6" src="https://kinsta.com/wp-content/themes/kinsta/images/home-migrations.svg" alt="Merging lines representing migrations" width="20" />Free migrations</li>
 	<li class="row nomargin nocol middle-xs"><img class="mr--6" src="https://kinsta.com/wp-content/themes/kinsta/images/home-speed.svg" alt="Three right chevrons representing server speed" width="20" />Ultimate speed</li>
 	<li class="row nomargin nocol middle-xs"><img class="mr--6" src="https://kinsta.com/wp-content/themes/kinsta/images/home-backups.svg" alt="Circular arrow with center dot representing backups" width="20" />Daily backups</li>
 	<li class="row nomargin nocol middle-xs"><img class="mr--6" src="https://kinsta.com/wp-content/themes/kinsta/images/home-infrastructure.svg" alt="Offset hexagons representing our server stack" width="20" />Google Cloud Platform</li>
</ul>
</div>
<div class="mt--10"><a class="button--purple button--small" href="https://kinsta.com/plans/?article-sidebar-promo">CHECK OUT OUR PLANS</a></div>
</div>
</div>
<div id="attachment_6179" class="wp-caption alignnone"><img class="alignnone size-full wp-image-2575" src="http://hss5.com/wp-content/uploads/2019/03/think-with-google-speed-test-1.jpg" width="1280" height="887" alt="think with google speed test" />
<p class="wp-caption-text">think with Google speed test</p>

</div>
As we are talking about Google Pagespeed Insights make sure you take a look at this detailed guide –&gt; <a href="https://kinsta.com/blog/google-pagespeed-insights/" target="_blank" rel="noopener">How to Score 100/100 in Google PageSpeed Insights with WordPress</a>
<h2 id="fix-leverage-browser-caching-wordpress">Fix the Leverage Browser Caching Warning in WordPress</h2>
When it comes to fixing the leverage browser caching warning there are a couple different scenarios that are usually encountered by WordPress users. Obviously, the most common one is that your web server is not correctly configured. The second irony is that the Google Analytic’s script gives us the warning. And the third is other third-party scripts returning the warning. See what your options are below.
<h3>1. Leverage Browser Caching on Server</h3>
The first and most common reason the leverage browser caching warning is triggered is that your web server doesn’t have the appropriate headers in place. In the screenshot below in Google PageSpeed Insights you will see the reason is that an <strong>expiration is not specified</strong>. When it comes to caching there are two primary methods which are used, <a href="https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers#http-cache-headers" target="_blank" rel="noopener noreferrer">Cache-Control headers and Expires headers</a>. While the Cache-Control header turns on client-side caching and sets the max-age of a resource, the Expires header is used to specify a specific point in time the resource is no longer valid.
<div id="attachment_6149" class="wp-caption alignnone"><img class="alignnone size-full wp-image-2576" src="http://hss5.com/wp-content/uploads/2019/03/leverage-browser-caching-pagespeed-insights-assets.png" width="1261" height="578" alt="leverage browser caching pagespeed insights assets" />
<p class="wp-caption-text">Leverage browser caching warning in Google PageSpeed Insights</p>

</div>
So now let’s explore how to add these headers to your web server. <strong>Note: You don’t necessarily need to add both of the headers, as this is a little redundant.</strong> Cache-Control is newer and usually the recommended method, however, some web performance tools like <a href="https://kinsta.com/blog/gtmetrix-speed-test/" target="_blank" rel="noopener noreferrer">GTmetrix</a> still check for Expires headers. These are all examples, you can change file types, expire times, etc. based on your needs.
<div class="warning">Important! Editing your Nginx config or Apache .htaccess file could break your site if not done correctly. If you are not comfortable doing this, please check with your web host or developer first.</div>
<h4 id="cache-control-header-nginx"><strong>Adding Cache-Control Header in Nginx</strong></h4>
You can add Cache-Control headers in Nginx by adding the following to your server config’s server location or block.
<pre><code>location ~* \.(js|css|png|jpg|jpeg|gif|svg|ico)$ {
 expires 30d;
 add_header Cache-Control "public, no-transform";
}</code></pre>
So what exactly is the code above does? Basically, it is telling the server that the file types are not going to change for at least one month. So instead of having to download the resource every time, it caches it on your computer. This way it is faster for return visits.
<h4><strong>Adding Expires Headers in Nginx</strong></h4>
You can add Expires headers in Nginx by adding the following to your server block. In this example, you can see how to specify different expire times based on file types.
<pre><code>    location ~*  \.(jpg|jpeg|gif|png|svg)$ {
        expires 365d;
    }

    location ~*  \.(pdf|css|html|js|swf)$ {
        expires 2d;
    }</code></pre>
<h4><strong>Adding Cache-Control Headers in Apache</strong></h4>
You can add Cache-Control headers in <a href="https://kinsta.com/knowledgebase/what-is-apache/" target="_blank" rel="noopener">Apache</a> by adding the following to your .htaccess file. Snippets of code can be added at the top or bottom of the file (before # BEGIN WordPress or after # END WordPress).
<pre><code>&lt;filesMatch ".(ico|pdf|flv|jpg|jpeg|png|gif|svg|js|css|swf)$"&gt;
Header set Cache-Control "max-age=84600, public"
&lt;/filesMatch&gt;</code></pre>
<h4><strong>Adding Expires Headers in Apache</strong></h4>
You can add Expires headers in Apache by adding the following to your .htaccess file.
<pre><code>## EXPIRES HEADER CACHING ##
&lt;IfModule mod_expires.c&gt;
ExpiresActive On
ExpiresByType image/jpg "access 1 year"
ExpiresByType image/jpeg "access 1 year"
ExpiresByType image/gif "access 1 year"
ExpiresByType image/png "access 1 year"
ExpiresByType image/svg "access 1 year"
ExpiresByType text/css "access 1 month"
ExpiresByType application/pdf "access 1 month"
ExpiresByType application/javascript "access 1 month"
ExpiresByType application/x-javascript "access 1 month"
ExpiresByType application/x-shockwave-flash "access 1 month"
ExpiresByType image/x-icon "access 1 year"
ExpiresDefault "access 2 days"
&lt;/IfModule&gt;
## EXPIRES HEADER CACHING ##</code></pre>
If you are a Kinsta client you don’t have to worry about adding these headers. These are already in place on all of our <a href="https://kinsta.com/knowledgebase/what-is-nginx/" target="_blank" rel="noopener">Nginx servers</a>. And remember, if you use CDN provider like <a href="https://kinsta.com/knowledgebase/kinsta-cdn/" target="_blank" rel="noopener">KeyCDN</a>, these headers are most likely already being set on your assets.

You can check your headers in Chrome DevTools network panel or simply be re-running your WordPress site through Google PageSpeed Insights again to ensure the warning is now gone.
<div class="in-post-container">
<div id="simple-promo">
<div class="mb--20 mt--0 heading--normal">Struggling with downtime and WordPress problems? Kinsta is the hosting solution designed to save you time! <a href="https://kinsta.com/features/">Check out our features</a></div>
</div>
</div>
<div id="attachment_6154" class="wp-caption alignnone"><img class="alignnone size-full wp-image-2577" src="http://hss5.com/wp-content/uploads/2019/03/caching-headers-wordpress.jpg" width="1280" height="705" alt="caching headers wordpress" />
<p class="wp-caption-text">HTTP caching headers</p>

</div>
<h3>2. Leverage Browser Caching and Google Analytics</h3>
The second most common leverage browser caching warning actually comes from Google Analytics. This is kind of ironic seeing as this is Google’s own script. The issue is that they set a low 2 hour cache time on their asset, as seen in the screenshot below. They most likely do this because if for some reason they were to modify something on there end they want all users to get the changes as fast as possible.  However there is a way to get around this, and that is by hosting Google Analytics script on your own server. Please be aware though that this is <a href="https://support.google.com/analytics/answer/1032389?hl=en" target="_blank" rel="noopener noreferrer">not supported by Google</a>.
<div id="attachment_6150" class="wp-caption alignnone"><img class="alignnone size-full wp-image-2578" src="http://hss5.com/wp-content/uploads/2019/03/leverage-browser-caching-pagespeed-insights-analytics.png" width="1265" height="452" alt="leverage browser caching-pagespeed insights analytics" />
<p class="wp-caption-text">Google Analytics caching</p>

</div>
There is a great free little plugin called <a href="https://wordpress.org/plugins/host-analyticsjs-local/" target="_blank" rel="noopener noreferrer">Complete Analytics Optimization Suite</a>, created and developed by Daan van den Bergh, which allows you to host Google Analytics locally on your WordPress website.
<div id="attachment_6155" class="wp-caption alignnone"><img class="alignnone size-full wp-image-2579" src="http://hss5.com/wp-content/uploads/2019/03/host-google-analytics-locally-plugin.jpg" width="1280" height="408" alt="host google analytics locally plugin" />
<p class="wp-caption-text">CAOS plugin</p>

</div>
You can download Complete Analytics Optimization Suite from the WordPress repository or by searching for it under “Add New” plugins in your WordPress dashboard. The plugin allows you to host your Google Analytics JavaScript file (analytics.js) locally and keep it updated using wp_cron(). Other features include being able to easily anonymize the IP address of your visitors, set an adjusted bounce rate, and placement of the script (header or footer).

Some additional benefits to hosting your analytics script locally is that you reduce your external HTTP requests to Google from 2 down to 1 and you now have <strong>full control over the caching of the file</strong>. This means you can utilize the cache headers as we showed you above.

Just install the plugin, enter your Google Analytics Tracking ID, and the plugin adds the necessary tracking code for Google Analytics to your WordPress website, downloads and saves the analytics.js file to your server and keeps it updated using a scheduled script in wp_cron(). We recommend also setting it to load in the footer. Note: This plugin won’t work with other Google Analytics WordPress plugins.
<div id="attachment_6157" class="wp-caption alignnone"><img class="alignnone size-full wp-image-2580" src="http://hss5.com/wp-content/uploads/2019/03/local-analytics-settings.jpg" width="1280" height="1062" alt="local analytics settings" />
<p class="wp-caption-text">Locally hosted analytics settings</p>

</div>
<h3>3. What About Other 3rd Party Scripts?</h3>
If you are running a business on your WordPress website, most likely you have additional <a href="https://kinsta.com/blog/identify-analyze-external-services-wordpress-site/" target="_blank" rel="noopener noreferrer">3rd party scripts</a> running to track conversions, <a href="https://kinsta.com/blog/wordpress-ab-testing-tools/" target="_blank" rel="noopener">A/B tests</a>, etc. This might include scripts like <a href="https://kinsta.com/blog/conversion-tracking/#fb-conversion-tracking" target="_blank" rel="noopener">Facebook conversion pixels</a>, Twitter, CrazyEgg, Hotjar, etc. Unfortunately, since you can’t host those locally there is nothing much that can be done as you don’t have control over the caching of those 3rd party assets. But for many smaller sites and bloggers, you most likely can get rid of that leverage browser caching warning altogether by following the recommendations above.
<div id="attachment_6152" class="wp-caption alignnone"><img class="alignnone size-full wp-image-2581" src="http://hss5.com/wp-content/uploads/2019/03/leverage-browser-caching-3rd-party-scripts.png" width="1263" height="567" alt="leverage browser caching 3rd party scripts" />
<p class="wp-caption-text">Leverage browser caching warning from 3rd party scripts</p>

</div>
<h2>Summary</h2>
You definitely have some options available to you when trying to fix that annoying leverage browser caching warning on your WordPress site. For most people, you can probably clear it up altogether. Remember, these web performance tools should be used as guidelines. We wouldn’t recommend obsessing over the scores too much. But fixing the warnings will usually result in a faster WordPress website in the end.

Have any other tips about fixing the leverage browser caching warning? If so, feel free to drop us a comment below.
<div class="essb_break_scroll"></div>
</div>
</div>
<div id="widget-share-6137" class="widget-share text--color js-stickybit-parent">
<div id="widget-share-aligner-6137" class="widget-share-aligner js-is-stuck">
<div class="widget-share-inner">
<div class="widget-share__total mb--10 mb--sm--0">
<div class="amount text--tiny mr--10">
<div class="share-icon text--color"></div>
</div>
</div>
</div>
</div>
</div>
<div class="user-content mt--40">If you enjoyed this article, then you'll love Kinsta’s WordPress hosting platform. Turbocharge your website and get 24x7 support from our veteran WordPress team. Our Google Cloud powered infrastructure focuses on auto-scaling, performance, and security. Let us show you the Kinsta difference! <a href="https://kinsta.com/plans/">Check out our plans</a></div>