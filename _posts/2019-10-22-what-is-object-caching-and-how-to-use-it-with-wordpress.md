---
ID: 3288
post_title: >
  What is Object Caching and How to Use It
  With WordPress
author: peng, chris
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/10/22/what-is-object-caching-and-how-to-use-it-with-wordpress/
published: true
post_date: 2019-10-22 20:28:10
---
https://wp-rocket.me/blog/object-caching-use-wordpress/

When it comes to caching, there are a lot of different types. There’s browser caching and page caching, not to mention mobile and user caching. If you’re using WP Rocket, you are no doubt already benefiting from these types of caching. But another one you’ll want to add to the mix is <strong>object caching</strong>.

Object caching involves storing database queries and, when enabled on your WordPress site, it can help speed up PHP execution times, reduce the load on your database, and deliver content to your visitors faster.

In this post, we’ll cover what object caching is and how it works with WordPress (including the built-in object caching that WordPress provides). We’ll also look at a few different ways you can implement this type of caching on your site to improve its performance.
<h2>WHAT IS CACHING?</h2>
But first, let’s look at what caching is generally to put object caching into context.

Caching is the process of storing frequently-accessed data temporarily in a cache so you can reuse it for subsequent requests.

When someone lands on your site, and you don’t have caching enabled, their browser sends a request for the page to your server. Your server then processes the request, compiles the page, and then sends it back to the browser.

If you have a low traffic site, your server can sit back and relax, so to speak, since it only has to process and compile pages every now and again. But servers hosting large sites have their work cut out for them—they have to work much harder to process and compile multiple pages per second as requests come in.

<img class="alignnone size-full wp-image-3289" src="https://www.hss5.com/wp-content/uploads/2019/10/how-servers-work.jpg" width="800" height="356" alt="how do servers work" />

This is where caching can help ease the load on your server. It stores a copy of each request and then the next time the same request comes it, it checks the cache and serves it from there. If there’s no copy, the request is sent on to the server to be processed and compiled, and on its way back to the browser a copy is stored in cache.

The beauty of caching is that it saves your server from having to do more work than it has to, allowing it to handle more traffic than it other would. It also has the added benefit of delivering your content faster to users.

If you’re interested to learn more about how caching works, check out <a tabindex="1" href="https://wp-rocket.me/blog/caching-plugin-is-critical-to-your-wordpress-site/" target="_blank" rel="noopener noreferrer">Why a Caching Plugin is Critical to Your WordPress Site</a>.
<h3>Different Types of Caching</h3>
There are two main types of caching: <strong>client-side caching</strong> and <strong>server-side caching</strong>.

There are many types of client-side caching, but the one you’re probably most familiar with is <a tabindex="1" href="https://wp-rocket.me/blog/browser-caching-explained-in-plain-english/" target="_blank" rel="noopener noreferrer">browser caching</a>. This is where the browser stores static web page content so the next time someone visits your site, the page is pulled from the cache on their computer instead of being downloaded again.

Object caching is a type of server-side caching. There are lots of types of server-side caching, but the important ones to know about include:

<strong>1. Object caching.</strong> We’ll go into this in more detail below, but object caching involves storing database queries so that the next time a piece of data is needed, it is delivered from cache without having to query the database.

<strong>2. <a tabindex="1" href="https://wp-rocket.me/blog/wordpress-page-caching-explained-in-plain-english/" target="_blank" rel="noopener noreferrer">Page caching</a>.</strong> Page caching involves storing the entire HTML of a page so that on subsequent views, the content—including files and database queries—can be generated and displayed without WordPress having to do it each time.

<strong>3. <a tabindex="1" href="https://docs.wp-rocket.me/article/673-what-is-opcache" target="_blank" rel="noopener noreferrer">Opcode caching</a>.</strong> Opcode caching involves compiling PHP code between every request. For PHP code to execute, the PHP compiler has to compile the code first and then generate executable code for the server to execute. Opcode caches the already compiled code.

<strong>4. <a tabindex="1" href="https://wp-rocket.me/blog/best-content-delivery-networks-wordpress-2017/" target="_blank" rel="noopener noreferrer">CDN caching</a>.</strong> Content delivery networks (CDNs) using edge servers around the world to store static website files (i.e., CSS, JavaScript, and media files) for faster delivery to users who are geographically distant from the host server.
<h2>SO WHAT IS OBJECT CACHING?</h2>
Object caching involves storing database query results so that the next time a result is needed, it can be served from the cache without having to repeatedly query the database.

As a content management system, WordPress is naturally—and heavily—dependent on the database. As such, database efficiency is crucial to scaling WordPress.

If you run a high-traffic site and requests to your pages generate a large number of database queries, your server can quickly become overwhelmed, in turn negatively affecting your site’s performance.

So when object caching is enabled on your site, it can help ease the load on your database and server and deliver queries faster.
<h3>What is WP_Object_Cache?</h3>
WordPress has a built-in object cache called <a tabindex="1" href="https://codex.wordpress.org/Class_Reference/WP_Object_Cache" target="_blank" rel="noopener noreferrer">WP_Object_Cache</a>. Introduced in 2005, it provides a way of automatically storing any data from the database in PHP memory to prevent repeated queries.

However, this object cache only stores objects for a single page load—it discards objects in cache at the end of the request, so they have to be rebuilt from scratch the next time the page is requested.

While this is a useful feature of WordPress, ensuring the database isn’t queried multiple times during a single page load for similar query requests, it’s not exactly efficient.

This is where persistent caching solutions can help. Object caching is more powerful when it can be used to cache objects between multiple page loads.

External persistent object caching solutions like Redis and Memcached make it possible to persist the object cache between requests. This helps speed up the delivery of database queries while further easing the workload of your server.
<h3>What are Redis and Memcached?</h3>
Persistent object caching is a must if you’re looking to scale. Without it, your site’s performance will slow down as its complexity and traffic increase. The same goes for logged in users and dynamic pages—object caching can help deliver a better and faster user experience.

There are two popular persistent object caching tools worth checking out: <strong>Redis</strong> and <strong>Memcached</strong>.

Both of these tools are fast and powerful in-memory data stores that can reduce the load on your site’s MySQL database, while also decreasing the response time of your site and bolstering your site’s ability to scale and handle increased traffic.

Memcached has long been a popular cache choice, but Redis can do everything Memcached can, and with a much larger feature set. Plus, it’s more popular and better supported.

For an in-depth look at the features, pros and cons of Redis and Memcached, <a tabindex="1" href="https://stackoverflow.com/questions/10558465/memcached-vs-redis" target="_blank" rel="noopener noreferrer">this Stack Overflow thread</a> has some great general information about both tools.
<h2>HOW TO USE OBJECT CACHING WITH WORDPRESS</h2>
The object caching built-in to WordPress is already working on your site by default, so you don’t need to do anything to enable it.

But if you want to take your object caching to the next level so your database queries are cached persistently between page loads, there are a few options available that are straightforward to implement.
<h3>1. Use Redis</h3>
For Redis-powered object caching, you can’t go past the free plugins available at <a tabindex="1" href="https://wordpress.org/" target="_blank" rel="noopener noreferrer">WordPress.org</a>.

With more than 30,000 active installs, the most popular option is <a tabindex="1" href="https://wordpress.org/plugins/redis-cache/" target="_blank" rel="noopener noreferrer">Redis Object Cache</a>. It supports Predis, PhpRedis (PECL), HHVM, replication, clustering and WP-CLI.

Before using this plugin, you’ll need to check that your site is using a PHP environment with the required PHP Redis extension and a working Redis server.

If you’re good to go, this plugin is super simple to install—just activate the plugin, go to <strong>Settings &gt; Redis</strong> and click “Enable Object Cache.”

<img class="alignnone size-full wp-image-3290" src="https://www.hss5.com/wp-content/uploads/2019/10/redis-object-cache-plugin.png" width="821" height="436" alt="Redis Object Cache" />

If you run into any problems, again, you’ll need to check with your web host whether the server your site is hosted on is set up for Redis. Since Cloudways hosts my test site, I had to log-in to the dashboard for my site and enabled Redis as a package before enabling object using in the Redis Object Cache plugin.

Alternatively, another option you might want to try is <a tabindex="1" href="https://wordpress.org/plugins/wp-redis/" target="_blank" rel="noopener noreferrer">WP Redis</a>, which is a little more involved to set up. Brought to you by the folks at web host Pantheon, this plugin requires that you create a file called object-cache.php and add it to your wp-content folder, and also edit your wp-config.php folder (but only if you’re not a Pantheon user).

If WP-CLI is a big part of your development workflow, you might find it more convenient to use WP Redis since it comes with a variety of commands.
<h3>2. Ask Your Host</h3>
If you’re on managed WordPress hosting, your host probably offers object caching via Redis. So check out your host’s documentation for information on how to enable object caching, or get in touch to check if it’s available.

Many hosts offer Redis as a feature or add-on—Pantheon, Kinsta and Cloudways, just to name a few.

If you’re on shared hosting, it’s less likely that it will be available for free. In this case, if object caching isn’t available to you, you might want to consider upgrading your hosting package or switching hosts.
<h2>WRAPPING UP</h2>
Object caching provides a relatively simple solution for improving the performance of your database, especially given the fact that WordPress performance is heavily dependent on the speed of your database.

With solutions like Redis, you can quickly enable persistent object caching on your site, whether it’s via a plugin or simply asking your host to activate it.

<em>Do you use object caching on your site? If you have any questions about how to use object caching, leave a comment below!</em>