---
ID: 3243
post_title: 'WooCommerce Visual Hook Guide: Single Product Page customize and easy code placement'
author: peng, chris
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/09/09/woocommerce-visual-hook-guide-single-product-page-customize-and-easy-code-placement/
published: true
post_date: 2019-09-09 16:01:49
---
<h1 class="entry-title">WooCommerce Visual Hook Guide: Single Product Page</h1>
&nbsp;
<pre>&lt;?php echo do_shortcode( '[ecp code="vtigerwebform"]' ); ?&gt;</pre>
<pre>add_action( 'woocommerce_after_single_product' , 'shortcode_vtigerwebform', 5 );
 
function shortcode_vtigerwebform() {
    echo do_shortcode( '[ecp code="vtigerwebform"]' );
}</pre>
https://www.000webhost.com/blog/wordpress-do_shortcode/

https://businessbloomer.com/woocommerce-add-content-below-the-single-product-page-images/

https://wordpress.org/plugins/easy-code-placement/

&nbsp;