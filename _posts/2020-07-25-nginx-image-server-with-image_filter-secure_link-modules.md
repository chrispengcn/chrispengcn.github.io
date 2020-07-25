---
ID: 3690
post_title: 'NGINX: Image Server with image_filter &#038; secure_link modules'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2020/07/25/nginx-image-server-with-image_filter-secure_link-modules/
published: true
post_date: 2020-07-25 12:00:33
---
https://zaiste.net/posts/nginx-image-server-image-filter-secure-link-modules/

NGINX with enabled <a href="http://nginx.org/en/docs/http/ngx_http_image_filter_module.html"><code>image_filter</code></a> and <a href="http://nginx.org/en/docs/http/ngx_http_secure_link_module.html"><code>secure_links</code></a> modules allows to quickly build an image server. The server can resize and cache images. It acts as a reverse-proxy between the external world and an internal storage with full-size image versions. Additionally, it protects images using MD5 hash values. It's a basic mechanism of resource authorization. It allows to access an image only if its hash value is known to the requester.

NGINX needs to be compiled with those two modules. This is not a default configuration. You can check enabled modules by running <code>nginx -V</code>. <code>nginx-extras</code> package in Ubuntu, however, comes with <code>image_filter</code> and <code>secure_links</code> already enabled.

Starting from NGINX 1.9.11 you can load modules dynamically. You can install the module and enable it. Here are Debian/Ubuntu specific instructions.
<pre><code>sudo apt-get install nginx-module-image-filter
</code></pre>
Then, in <code>nginx.conf</code> use <code>load_module</code> directive to enable it:
<pre><code>load_module modules/ngx_http_image_filter_module.so;
</code></pre>
Let's start with the cache. It is a file-based mechanism that stores resized versions of images. It should be large enough to reduce the number of resizing operations. In our example, the cache will be holding up to 10 GB of images. The <code>max_size</code> parameter defines the point at which NGINX starts to remove the least recently requested images from the cache to make room for new items.
<pre><code>proxy_cache_path /var/cache/nginx/images levels=1:2 keys_zone=images:64M inactive=60d max_size=10GB;
</code></pre>
Let's specify two <code>server</code> blocks: an internal server for resizing images acting as a reverse-proxy to the actual storage (in our case S3) and a public server that proxies authorized requests to the internal server and stores requested images in the cache.

Here's the public server:
<pre><code>server {
  listen 80;
  server_name images.zaiste.net;

  location /resize {
    secure_link $arg_hash;
    secure_link_md5 "$uri your-secret-goes-here";
    if ($secure_link = "") {
      return 404;
    }

    proxy_cache images;
    proxy_cache_lock on;
    proxy_cache_valid 30d;
    proxy_cache_use_stale error timeout invalid_header updating;
    expires 30d;
    proxy_pass http://localhost:11337;
  }
}
</code></pre>
The <code>secure_link</code> module works by generating a hash, which is the concatenation of the URL of the requested image and a secret string. This hash is long, so it is more convenient to append it to the URL rather than prepending it. This mechanisms prevents people from requesting arbitrary images.

Here's an example of a valid URL:
<pre><code>https://images.zaiste.net/resize/100x100/polyconf.jpg?hash=159c3isu11W1TBRm0YwBN
</code></pre>
Internal server fetches images from S3 and resizes them on-the-fly based on dimensions specified in the URL. You can also define other image transformation such us cropping or rotating.
<pre><code>server {
  listen 11337;
  server_name localhost;

  set $backend 's3.eu-central-1.amazonaws.com/your-bucket-name';

  resolver 8.8.8.8;
  resolver_timeout 30s;

  proxy_buffering off;
  proxy_pass_request_body off;
  proxy_pass_request_headers off;

  proxy_hide_header "x-amz-id-2";
  proxy_hide_header "x-amz-request-id";
  proxy_hide_header "x-amz-storage-class";
  proxy_hide_header "Set-Cookie";
  proxy_ignore_headers "Set-Cookie";

  proxy_set_header Host $backend;

  image_filter_jpeg_quality 95;
  image_filter_buffer 50M;
  image_filter_interlace on;

  location ~ ^/resize/(?&lt;width&gt;\d+)x(?&lt;height&gt;\d+)/(?&lt;name&gt;.*)$ {
    image_filter resize $width $height;
    proxy_pass http://$backend/$name;
  }

  location ~ ^/resize/(?&lt;width&gt;\d+)/(?&lt;name&gt;.*)$ {
    image_filter resize $width -;
    proxy_pass http://$backend/$name;
  }

 location ~ ^/crop/(?&lt;width&gt;\d+)x(?&lt;height&gt;\d+)/(?&lt;name&gt;.*)$ {
    image_filter crop $width $height;
    proxy_pass http://$backend/$name;
  }
}
</code></pre>
NGINX needs an explicit definition for DNS resolution when using variables in <code>proxy_pass</code> directive i.e. <code>$backend</code>. Otherwise, you will get <code>no resolver defined to resolve localhost</code> error.

You can clean AWS specific headers using <code>proxy_hide_header</code>.

Original images may be large. We need to allocate enough memory for <code>image_filter</code> module so it can load and resize them. In our example we set the <code>image_filter_buffer</code> directive to handle image files of up to 50 MB. The dash <code>‑</code> in the second example tells NGINX to maintain the original aspect ratio.

This setup, if used in production, should also provide a rate limiting mechanism. This can be done in NGINX using <a href="http://nginx.org/en/docs/http/ngx_http_limit_req_module.html"><code>limit_req</code></a> module.