---
ID: 3235
post_title: ADDING WEBP SUPPORT FOR NGINX
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/08/28/adding-webp-support-for-nginx/
published: true
post_date: 2019-08-28 16:45:58
---
This is a very short snippet post so you need to have knowledge about how to configure and customize your NGiNX setup since this is a quick mashup of the config required.

Install WebP.
<div class="highlighter-rouge">
<div class="highlight">
<pre class="highlight"><code>sudo apt-get install webp
</code></pre>
</div>
</div>
In http {} add the following:
<div class="highlighter-rouge">
<div class="highlight">
<pre class="highlight"><code>map $http_accept $webp_suffix {
  default   "";
  "~*webp"  ".webp";
}
</code></pre>
</div>
</div>
This mapping checks whether the browser client supports WebP images, when it does it will set WebP image files as default source.

In location / {} under the server {} add the following:
<div class="highlighter-rouge">
<div class="highlight">
<pre class="highlight"><code>location ~* ^.+\.(jpe?g|png) {
  add_header Cache-Control "public, no-transform";
  add_header Vary "Accept-Encoding";
  try_files $uri$webp_suffix $uri =404;
  expires max;
}
</code></pre>
</div>
</div>
For all jpg, jpeg or png images it will load the WebP variant of it if available, only if the browser client supports it.

I use the following script to convert all jpg, jpeg and png images to WebP while keeping the original in place for browser clients that doesn’t support WebP.
<div class="highlighter-rouge">
<div class="highlight">
<pre class="highlight"><code>echo "===== Converting images into WebP .webp ====="
for x in `find /var/www/itchy -type f \( -iname \*.jpeg -o -iname \*.jpg -o -iname \*.JPG -o -iname \*.png \)`
  do cwebp -quiet -q 80 ${x} -o ${x}.webp;
  echo "Converted $x to ${x}.webp"
done</code>


</pre>
https://itchy.nl/webp-support-for-nginx

</div>
</div>