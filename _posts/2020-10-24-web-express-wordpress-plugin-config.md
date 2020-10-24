---
ID: 3771
post_title: web express wordpress plugin config
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2020/10/24/web-express-wordpress-plugin-config/
published: true
post_date: 2020-10-24 14:10:51
---
<!-- wp:image {"id":3772,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://www.hss5.com/wp-content/uploads/2020/10/webp-express.png" alt="" class="wp-image-3772"/><figcaption><br><br></figcaption></figure>
<!-- /wp:image -->

<!-- wp:code -->
<pre class="wp-block-code"><code># nginx config 
# WebP Express rules
# --------------------
location ~* ^/?wp-content/.*\.(png|jpe?g)$ {
  add_header Vary Accept;
  expires 365d;
  if ($http_accept !~* "webp"){
    break;
  }
  try_files
    /wp-content/webp-express/webp-images/doc-root/$uri.webp
    $uri.webp
    /wp-content/plugins/webp-express/wod/webp-on-demand.php?xsource=x$request_filename&amp;wp-content=wp-content
    ;
}

# Route requests for non-existing webps to the converter
location ~* ^/?wp-content/.*\.(png|jpe?g)\.webp$ {
    try_files
      $uri
      /wp-content/plugins/webp-express/wod/webp-realizer.php?xdestination=x$request_filename&amp;wp-content=wp-content
      ;
}
# ------------------- (WebP Express rules ends here)</code></pre>
<!-- /wp:code -->

<!-- wp:core-embed/wordpress {"url":"https://wordpress.org/plugins/webp-express/#%0Ai%20am%20on%20nginx%20or%20openresty%0A","type":"wp-embed","providerNameSlug":"plugin-directory","className":""} -->
<figure class="wp-block-embed-wordpress wp-block-embed is-type-wp-embed is-provider-plugin-directory"><div class="wp-block-embed__wrapper">
https://wordpress.org/plugins/webp-express/#%0Ai%20am%20on%20nginx%20or%20openresty%0A
</div></figure>
<!-- /wp:core-embed/wordpress -->