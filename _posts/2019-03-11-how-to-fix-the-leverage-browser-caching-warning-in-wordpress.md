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
How to Fix the Leverage Browser Caching Warning in WordPress
By Brian Jackson • Updated on November 06, 2018
If you have ever run your WordPress website through Google PageSpeed Insights or Pingdom then you have probably seen that big yellow Leverage Browser Caching warning. And that is probably why you ended up at this post. Today we will dive into what this warning means, how it affects you, and what your options are as it pertains to your WordPress site.

What is the Leverage Browser Caching Warning?
Fix the Leverage Browser Caching Warning in WordPress
What is the Leverage Browser Caching Warning?
The leverage browser caching warning, as shown below in the screenshot, is referring to your browser cache. Whenever you visit a website, it downloads assets, such as HTML, CSS, JavaScript and images into your browser’s local cache. This way it doesn’t have to retrieve them on every page load. The warning itself is returned when your web server, or a third-party server, doesn’t have the correct HTTP cache headers implemented. Or the headers might exist, but the cache time is set too short.

leverage browser caching pagespeed insights
Set an expiry date or age in HTTP headers

You might also see this warning in the new “think with Google” website speed test, which is powered by Google PageSpeed Insights. This was designed to be more of a marketing tool by Google, but this has resulted in lot of clients now simply forwarding these errors to their webmasters. Leaving WordPress developers and designers looking for ways to quickly fix it to appease their clients.

Still looking for that perfect WordPress host?
Try Kinsta's premium managed WordPress hosting to experience your site without problems.
Styleized controls representing management Fully managed
Shield with a tick representing securitySecure like Fort Knox
Merging lines representing migrationsFree migrations
Three right chevrons representing server speedUltimate speed
Circular arrow with center dot representing backupsDaily backups
Offset hexagons representing our server stackGoogle Cloud Platform
CHECK OUT OUR PLANS
think with google speed test
think with Google speed test

As we are talking about Google Pagespeed Insights make sure you take a look at this detailed guide –> How to Score 100/100 in Google PageSpeed Insights with WordPress

Fix the Leverage Browser Caching Warning in WordPress
When it comes to fixing the leverage browser caching warning there are a couple different scenarios that are usually encountered by WordPress users. Obviously, the most common one is that your web server is not correctly configured. The second irony is that the Google Analytic’s script gives us the warning. And the third is other third-party scripts returning the warning. See what your options are below.

1. Leverage Browser Caching on Server
The first and most common reason the leverage browser caching warning is triggered is that your web server doesn’t have the appropriate headers in place. In the screenshot below in Google PageSpeed Insights you will see the reason is that an expiration is not specified. When it comes to caching there are two primary methods which are used, Cache-Control headers and Expires headers. While the Cache-Control header turns on client-side caching and sets the max-age of a resource, the Expires header is used to specify a specific point in time the resource is no longer valid.

leverage browser caching pagespeed insights assets
Leverage browser caching warning in Google PageSpeed Insights

So now let’s explore how to add these headers to your web server. Note: You don’t necessarily need to add both of the headers, as this is a little redundant. Cache-Control is newer and usually the recommended method, however, some web performance tools like GTmetrix still check for Expires headers. These are all examples, you can change file types, expire times, etc. based on your needs.

Important! Editing your Nginx config or Apache .htaccess file could break your site if not done correctly. If you are not comfortable doing this, please check with your web host or developer first.
Adding Cache-Control Header in Nginx
You can add Cache-Control headers in Nginx by adding the following to your server config’s server location or block.

location ~* \.(js|css|png|jpg|jpeg|gif|svg|ico)$ {
 expires 30d;
 add_header Cache-Control "public, no-transform";
}
So what exactly is the code above does? Basically, it is telling the server that the file types are not going to change for at least one month. So instead of having to download the resource every time, it caches it on your computer. This way it is faster for return visits.

Adding Expires Headers in Nginx
You can add Expires headers in Nginx by adding the following to your server block. In this example, you can see how to specify different expire times based on file types.

    location ~*  \.(jpg|jpeg|gif|png|svg)$ {
        expires 365d;
    }

    location ~*  \.(pdf|css|html|js|swf)$ {
        expires 2d;
    }
Adding Cache-Control Headers in Apache
You can add Cache-Control headers in Apache by adding the following to your .htaccess file. Snippets of code can be added at the top or bottom of the file (before # BEGIN WordPress or after # END WordPress).

<filesMatch ".(ico|pdf|flv|jpg|jpeg|png|gif|svg|js|css|swf)$">
Header set Cache-Control "max-age=84600, public"
</filesMatch>
Adding Expires Headers in Apache
You can add Expires headers in Apache by adding the following to your .htaccess file.

## EXPIRES HEADER CACHING ##
<IfModule mod_expires.c>
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
</IfModule>
## EXPIRES HEADER CACHING ##
If you are a Kinsta client you don’t have to worry about adding these headers. These are already in place on all of our Nginx servers. And remember, if you use CDN provider like KeyCDN, these headers are most likely already being set on your assets.

You can check your headers in Chrome DevTools network panel or simply be re-running your WordPress site through Google PageSpeed Insights again to ensure the warning is now gone.

Struggling with downtime and WordPress problems? Kinsta is the hosting solution designed to save you time! Check out our features
caching headers wordpress
HTTP caching headers

2. Leverage Browser Caching and Google Analytics
The second most common leverage browser caching warning actually comes from Google Analytics. This is kind of ironic seeing as this is Google’s own script. The issue is that they set a low 2 hour cache time on their asset, as seen in the screenshot below. They most likely do this because if for some reason they were to modify something on there end they want all users to get the changes as fast as possible.  However there is a way to get around this, and that is by hosting Google Analytics script on your own server. Please be aware though that this is not supported by Google.

leverage browser caching-pagespeed insights analytics
Google Analytics caching

There is a great free little plugin called Complete Analytics Optimization Suite, created and developed by Daan van den Bergh, which allows you to host Google Analytics locally on your WordPress website.

host google analytics locally plugin
CAOS plugin

You can download Complete Analytics Optimization Suite from the WordPress repository or by searching for it under “Add New” plugins in your WordPress dashboard. The plugin allows you to host your Google Analytics JavaScript file (analytics.js) locally and keep it updated using wp_cron(). Other features include being able to easily anonymize the IP address of your visitors, set an adjusted bounce rate, and placement of the script (header or footer).

Some additional benefits to hosting your analytics script locally is that you reduce your external HTTP requests to Google from 2 down to 1 and you now have full control over the caching of the file. This means you can utilize the cache headers as we showed you above.

Just install the plugin, enter your Google Analytics Tracking ID, and the plugin adds the necessary tracking code for Google Analytics to your WordPress website, downloads and saves the analytics.js file to your server and keeps it updated using a scheduled script in wp_cron(). We recommend also setting it to load in the footer. Note: This plugin won’t work with other Google Analytics WordPress plugins.

local analytics settings
Locally hosted analytics settings

3. What About Other 3rd Party Scripts?
If you are running a business on your WordPress website, most likely you have additional 3rd party scripts running to track conversions, A/B tests, etc. This might include scripts like Facebook conversion pixels, Twitter, CrazyEgg, Hotjar, etc. Unfortunately, since you can’t host those locally there is nothing much that can be done as you don’t have control over the caching of those 3rd party assets. But for many smaller sites and bloggers, you most likely can get rid of that leverage browser caching warning altogether by following the recommendations above.

leverage browser caching 3rd party scripts
Leverage browser caching warning from 3rd party scripts

Summary
You definitely have some options available to you when trying to fix that annoying leverage browser caching warning on your WordPress site. For most people, you can probably clear it up altogether. Remember, these web performance tools should be used as guidelines. We wouldn’t recommend obsessing over the scores too much. But fixing the warnings will usually result in a faster WordPress website in the end.

Have any other tips about fixing the leverage browser caching warning? If so, feel free to drop us a comment below.

306
Shares
If you enjoyed this article, then you'll love Kinsta’s WordPress hosting platform. Turbocharge your website and get 24x7 support from our veteran WordPress team. Our Google Cloud powered infrastructure focuses on auto-scaling, performance, and security. Let us show you the Kinsta difference!