---
ID: 3665
post_title: >
  How can I manually specify the memcached
  server(s)?
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2020/06/09/how-can-i-manually-specify-the-memcached-servers/
published: true
post_date: 2020-06-09 16:58:49
---
Add something similar to the following to wp-config.php above <code>/* That's all, stop editing! Happy blogging. */</code>:
<pre><code>$memcached_servers = array(
    'default' =&gt; array(
        '10.10.10.20:11211',
        '10.10.10.30:11211'
    )
);
</code></pre>
The top level array keys, are cache groups, where ‘default’ corresponds to any cache group that is not explicitly defined. This allows for specifying memcached servers that only handle certain cache groups. The most common use is only specifying ‘default’.

Possible cache groups are:
<pre><code>{$taxonomy}_relationships
{$meta_type}_meta
{$taxonomy}_relationships
blog-details
blog-id-cache
blog-lookup
bookmark
calendar
category
comment
counts
general
global-posts
options
plugins
post_ancestors
post_meta
posts
rss
site-lookup
site-options
site-transient
terms
themes
timeinfo
transient
user_meta
useremail
userlogins
usermeta
users
userslugs
widget</code></pre>