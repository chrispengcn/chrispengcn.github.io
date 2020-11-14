---
ID: 3797
post_title: nginx pagespeed config
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://vpseo.com/2020/11/15/nginx-pagespeed-config/
published: true
post_date: 2020-11-15 02:34:26
---
<pre># Enable pagespeed module by putting the following in http context
pagespeed on;

pagespeed Domain example.com;

pagespeed DownstreamCachePurgeLocationPrefix http://lb1.i;
pagespeed DownstreamCachePurgeMethod PURGE;
pagespeed DownstreamCacheRewrittenPercentageThreshold 95;

# This setting should be enabled when using HTTPS
# Take care when using HTTP &gt; HTTPS redirection to avoid loops
# pagespeed MapOriginDomain http://example.com https://example.com;

pagespeed EnableFilters extend_cache;

pagespeed Statistics on;
pagespeed StatisticsLogging on;
pagespeed LogDir /var/log/pagespeed;

# Needs to exist and be writable by nginx.  Use tmpfs for best performance.
pagespeed FileCachePath /var/ngx_pagespeed_cache;

pagespeed RespectVary on;
pagespeed LowercaseHtmlNames on;

# optional
pagespeed XHeaderValue "Powered By ngx_pagespeed";

# admin

# filter
pagespeed RewriteLevel PassThrough;

# image
pagespeed EnableFilters lazyload_images;
pagespeed LazyloadImagesAfterOnload off;
pagespeed LazyloadImagesBlankUrl "https://www.gstatic.com/psa/static/1.gif";
pagespeed EnableFilters rewrite_images,responsive_images_zoom;
pagespeed DisableFilters convert_jpeg_to_webp,convert_to_webp_lossless,convert_to_webp_animated,recompress_webp;

# css
pagespeed EnableFilters inline_import_to_link;
pagespeed EnableFilters outline_css;
pagespeed CssOutlineMinBytes 1000;
pagespeed EnableFilters combine_css,rewrite_css,rewrite_style_attributes,flatten_css_imports,prioritize_critical_css,sprite_images;

# js
pagespeed EnableFilters rewrite_javascript,combine_javascript;
pagespeed UseExperimentalJsMinifier on;

pagespeed EnableFilters remove_comments;
pagespeed EnableFilters collapse_whitespace;
pagespeed EnableFilters elide_attributes;
pagespeed EnableFilters extend_cache;
pagespeed EnableFilters trim_urls;

pagespeed EnableFilters local_storage_cache;

pagespeed MemcachedThreads 1;
pagespeed MemcachedServers "10.130.94.158:11211";
pagespeed MemcachedTimeoutUs 100000;

pagespeed StatisticsPath /ngx_pagespeed_statistics;
pagespeed GlobalStatisticsPath /ngx_pagespeed_global_statistics;
pagespeed MessagesPath /ngx_pagespeed_message;
pagespeed ConsolePath /pagespeed_console;
pagespeed AdminPath /pagespeed_admin;
pagespeed GlobalAdminPath /pagespeed_global_admin;

#https://gist.github.com/hendra/e90e17067ea5d4c114396c3c4ed74262</pre>