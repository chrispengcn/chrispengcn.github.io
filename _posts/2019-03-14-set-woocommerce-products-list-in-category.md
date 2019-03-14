---
ID: 2605
post_title: >
  自定义WooCommerce每页显示的产品数量
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/03/14/set-woocommerce-products-list-in-category/
published: true
post_date: 2019-03-14 20:02:51
---
<h3>自定义WooCommerce每页显示的产品数量</h3>
WooCommerce默认显示为10，为了美观，往往我们要多显示些产品数量。因此，只

要把如下代码<strong>copy到你主题functions.php文件中即可</strong>。
<div class="gap"></div>
<div class="shortcode-code">

/**
* 定义woocomerce每页显示的产品数量
*/
add_filter( 'loop_shop_per_page', create_function( '$cols', 'return 14;' ), 20 );

</div>
<div class="gap"></div>
<strong>return 14;，14修改成你需要显示的数量。效果如图：</strong>
<div class="gap"></div>
<img class="alignnone size-full wp-image-2608" title="自定义WooCommerce每页显示的产品数量" src="http://hss5.com/wp-content/uploads/2019/03/canvas.png" alt="canvas 自定义WooCommerce每页显示的产品数量" width="946" height="1280" />