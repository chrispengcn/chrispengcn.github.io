---
ID: 3590
post_title: Memcached Object Cache for wordpress
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2020/05/25/memcached-object-cache-for-wordpress/
published: true
post_date: 2020-05-25 13:06:45
---
<h2 id="installation-header">Installation</h2>
<ol>
 	<li>Install <a href="http://danga.com/memcached" rel="nofollow ugc">memcached</a> on at least one server. Note the connection info. The default is <code>127.0.0.1:11211</code>.</li>
 	<li>Install the <a href="http://pecl.php.net/package/memcache" rel="nofollow ugc">PECL memcache extension</a></li>
 	<li>Copy object-cache.php to wp-content</li>
 	<li>Add the <code>WP_CACHE_KEY_SALT</code> constant to the <code>wp-config.php</code>:

<code>php
define('WP_CACHE_KEY_SALT', '...long random string...');</code></li>
</ol>
This helps prevent cache pollution when multiplte WordPress installs are using the same Memcached server. The value must be unique for each WordPress install.

== Frequently Asked Questions ==

= How can I manually specify the memcached server(s)? =

Add something similar to the following to wp-config.php above `/* That's all, stop editing! Happy blogging. */`:

`
$memcached_servers = array(
'default' =&gt; array(
'10.10.10.20:11211',
'10.10.10.30:11211'
)
);
`

&nbsp;