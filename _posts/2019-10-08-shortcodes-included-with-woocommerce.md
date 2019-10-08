---
ID: 3254
post_title: Shortcodes included with WooCommerce
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/10/08/shortcodes-included-with-woocommerce/
published: true
post_date: 2019-10-08 14:16:28
---
<div class="entry">

WooCommerce comes with several shortcodes that can be used to insert content inside posts and pages.
<div class="wistia_responsive_padding">
<div class="wistia_responsive_wrapper">
<div class="wistia_video_foam_dummy" data-source-container-id=""></div>
<iframe class="wistia_embed" title="Shortcodes Video" src="https://fast.wistia.net/embed/iframe/ouw5s9klok?dnt=1&amp;videoFoam=true" name="wistia_embed" width="660" height="371" frameborder="0" scrolling="no" allowfullscreen="allowfullscreen" data-mce-fragment="1"></iframe></div>
</div>
<h2 id="section-1">Page Shortcodes</h2>
<ul>
 	<li><strong>[woocommerce_cart]</strong> – shows the cart page</li>
 	<li><strong>[woocommerce_checkout]</strong> – shows the checkout page</li>
 	<li><strong>[woocommerce_my_account]</strong> – shows the user account page</li>
 	<li><strong>[woocommerce_order_tracking]</strong> – shows the order tracking form</li>
</ul>
In most cases, these shortcodes will be added to pages automatically via our <a href="https://docs.woocommerce.com/document/woocommerce-onboarding-wizard/">onboarding wizard</a> and do not need to be used manually.
<h3 id="section-2">Cart</h3>
Used on the cart page, the cart shortcode displays cart content and interface for coupon codes and other cart bits and pieces.

Args: none
<pre>[woocommerce_cart]</pre>
<h3 id="section-3">Checkout</h3>
Used on the checkout page, the checkout shortcode displays the checkout process.

Args: none
<pre>[woocommerce_checkout]</pre>
<h3 id="section-4">My Account</h3>
Shows the ‘my account’ section where the customer can view past orders and update their information. You can specify the number of orders to show. By default, it’s set to 15 (use <strong>-1</strong> to display <strong>all orders</strong>.)

Args:
<pre>array(
     'current_user' =&gt; ''
 )</pre>
<pre>[woocommerce_my_account]</pre>
<div class="woo-sc-box info   ">Current user argument is automatically set using get_user_by( ‘id’, get_current_user_id() ).</div>
<h3 id="section-5">Order Tracking Form</h3>
Lets a user see the status of an order by entering their order details.

Args: none
<pre>[woocommerce_order_tracking]</pre>
<h2 id="section-6">Products</h2>
The products shortcode is one of our most robust shortcodes, which can replace various other strings used in earlier versions of WooCommerce. For WooCommerce version 3.1 and lower, <a href="https://docs.woocommerce.com/document/woocommerce-shortcodes/3-1-earlier/" target="_blank" rel="noopener noreferrer">please use this document instead</a>.
The <code>[products]</code> shortcode allows you to display products by post ID, SKU, categories, attributes, with support for pagination, random sorting and product tags, replacing the need for multiples shortcodes such as:  <code></code>,  <code>[featured_products]</code>, <code>[sale_products]</code>, <code>[best_selling_products]</code>, <code>[recent_products]</code>, <code>[product_attribute]</code>, and <code>[top_rated_products]</code>, which are needed in versions of WooCommerce below 3.2. Review the examples below.
<h3 id="section-7">Available Product Attributes</h3>
The following attributes are available to use in conjunction with the <code>[products]</code> shortcode. They have been split into sections for primary function for ease of navigation, with examples below.
<h4 id="display-product-attributes">Display Product Attributes</h4>
<ul>
 	<li><code>limit</code> – The number of products to display. Defaults to and <code>-1</code> (display all)  when listing products, and <code>-1</code> (display all) for categories.</li>
 	<li><code>columns</code> – The number of columns to display. Defaults to <code>4</code>.</li>
 	<li><code>paginate</code> – Toggles pagination on. Use in conjunction with <code>limit</code>. Defaults to <code>false</code> set to <code>true</code> to paginate .</li>
 	<li><code>orderby</code> – Sorts the products displayed by the entered option. One or more options can be passed by adding both slugs with a space between them. Available options are:
<ul>
 	<li><code>date</code> – The date the product was published.</li>
 	<li><code>id</code> – The post ID of the product.</li>
 	<li><code>menu_order</code> – The Menu Order, if set (lower numbers display first).</li>
 	<li><code>popularity</code> – The number of purchases.</li>
 	<li><code>rand</code> – Randomly order the products on page load (may not work with sites that use caching, as it could save a specific order).</li>
 	<li><code>rating</code> – The average product rating.</li>
 	<li><code>title</code> – The product title. This is the default <code>orderby</code> mode.</li>
</ul>
</li>
 	<li><code>skus</code> – Comma separated list of product SKUs.</li>
 	<li><code>category</code> – Comma separated list of category slugs.</li>
 	<li><code>tag</code> – Comma separated list of tag slugs.</li>
 	<li><code>order</code> – States whether the product ordering is ascending (<code>ASC</code>) or descending (<code>DESC</code>), using the method set in <code>orderby</code>. Defaults to <code>ASC</code>.</li>
 	<li><code>class</code> – Adds an HTML wrapper class so you can modify the specific output with custom CSS.</li>
 	<li><code>on_sale</code> – Retrieve on sale products. Not to be used in conjunction with <code>best_selling</code>or <code>top_rated</code>.</li>
 	<li><code>best_selling</code> – Retrieve best selling products. Not to be used in conjunction with <code>on_sale</code> or <code>top_rated</code>.</li>
 	<li><code>top_rated</code> – Retrieve top rated products. Not to be used in conjunction with <code>on_sale</code>or <code>best_selling</code>.</li>
</ul>
<h4 id="content-product-attributes">Content Product Attributes</h4>
<ul>
 	<li><code>attribute</code> – Retrieves products using the specified attribute slug.</li>
 	<li><code>terms</code> – Comma separated list of attribute terms to be used with <code>attribute</code>.</li>
 	<li><code>terms_operator</code> – Operator to compare attribute terms. Available options are:
<ul>
 	<li><code>AND</code> – Will display products from all of the chosen attributes.</li>
 	<li><code>IN</code> – Will display products with the chosen attribute. This is the default <code>terms_operator</code> value.</li>
 	<li><code>NOT IN</code> – Will display products that are not in the chosen attributes.</li>
</ul>
</li>
 	<li><code>tag_operator</code> – Operator to compare tags. Available options are:
<ul>
 	<li><code>AND</code> – Will display products from all of the chosen tags.</li>
 	<li><code>IN</code> – Will display products with the chosen tags. This is the default <code>tag_operator</code> value.</li>
 	<li><code>NOT IN</code> – Will display products that are not in the chosen tags.</li>
</ul>
</li>
 	<li><code>visibility</code> – Will display products based on the selected visibility. Available options are:
<ul>
 	<li><code>visible</code> – Products visibile on shop and search results. This is the default <code>visibility</code> option.</li>
 	<li><code>catalog</code> – Products visible on the shop only, but not search results.</li>
 	<li><code>search</code> – Products visible in search results only, but not on the shop.</li>
 	<li><code>hidden</code> – Products that are hidden from both shop and search, accessible only by direct URL.</li>
 	<li><code>featured</code> – Products that are marked as Featured Products.</li>
</ul>
</li>
</ul>
<ul>
 	<li><code>category</code> – Retrieves products using the specified category slug.</li>
 	<li><code>tag</code> – Retrieves products using the specified tag slug.</li>
 	<li><code>cat_operator</code> – Operator to compare category terms. Available options are:
<ul>
 	<li><code>AND</code> – Will display products that belong in all of the chosen categories.</li>
 	<li><code>IN</code> – Will display products within the chosen category. This is the default <code>cat_operator</code> value.</li>
 	<li><code>NOT IN</code> – Will display products that are not in the chosen category.</li>
</ul>
</li>
</ul>
<ul>
 	<li><code>ids</code> – Will display products based off of a comma separated list of Post IDs.</li>
 	<li><code>skus</code> – Will display products based off of a comma separated list of SKUs.</li>
</ul>
<em>*If the product is not showing, make sure it is not set to Hidden in the Catalog Visibility.</em>
<div class="woo-sc-box info   ">To find the Product ID, go to the <strong>Products </strong>screen, hover over the product and the ID appears as shown below.</div>
<img class="alignnone size-full wp-image-3255" src="https://www.hss5.com/wp-content/uploads/2019/10/Find-Product-ID-in-WooCommerce-950x281.png" width="950" height="281" alt="WooCommerce-Product-ID" />
<h4 id="special-product-attributes">Special Product Attributes</h4>
These attributes cannot be used with the “Content Attributes” listed above, as they will likely cause a conflict and not display. You should only use one of the following special attributes.
<ul>
 	<li><code>best_selling</code> – Will display your best selling products. Must be set to <code>true</code>.</li>
 	<li><code>on_sale</code> – Will display your on-sale products. Must be set to <code>true</code>.</li>
</ul>
<h4 id="product-category-shortcodes">Product Category shortcodes</h4>
These two shortcodes will display your product categories on any page.
<ul>
 	<li><code>[product_category]</code> – Will display products in a specified product category.</li>
 	<li><code>[product_categories]</code> – Will display all your product categories.</li>
</ul>
<h4 id="product-category-attributes">Product Category attributes</h4>
<ul>
 	<li><code>ids</code> – Specify specific category ids to be listed</li>
 	<li><code>limit</code> – The number of categories to display</li>
 	<li><code>columns</code> – The number of columns to display. Defaults to 4</li>
 	<li><code>hide_empty</code> – The default is “1” which will hide empty categories. Set to “0” to show empty categories</li>
 	<li><code>parent</code> – Set to a specific category ID if you would like to display all the child categories</li>
 	<li><code>orderby</code> – The default is to order by “name”, can be set to “id”, “slug”, or “menu_order”. If you want to order by the ids you specified then you can use <code>orderby="include"</code></li>
 	<li><code>order</code> – States whether the category ordering is ascending (<code>ASC</code>) or descending (<code>DESC</code>), using the method set in <code>orderby</code>. Defaults to <code>ASC</code>.</li>
</ul>
<h3 id="section-8">Example Product Scenarios</h3>
In the following scenarios, we’ll use an example clothing store.
<h4 id="scenario-1-random-sale-items">Scenario 1 – Random Sale Items</h4>
I want to display four random on sale products.
<pre>[products limit="4" columns="4" orderby="popularity" class="quick-sale" on_sale="true" ]</pre>
This shortcode explicity states four products with four columns (which will be one row), showing the most popular on-sale items. It also adds a CSS class <code>quick-sale</code>, which I can modify in my theme.

<img class="alignnone size-full wp-image-3256" src="https://www.hss5.com/wp-content/uploads/2019/10/shortcode-sale.png" width="807" height="374" alt="WooCommerce Shortcode - Sale Products" title="WooCommerce Shortcode – Sale Products" />
<h4 id="scenario-2-featured-products">Scenario 2 – Featured Products</h4>
I want to display my featured products, two per row, with a maximum of four items.
<pre>[products limit="4" columns="2" visibility="featured" ]</pre>
This shortcode says up to four products will load in two columns, and that they must be featured. Although not explicitly stated, it uses the defaults such as sorting by title (A to Z).

<img class="alignnone size-full wp-image-3257" src="https://www.hss5.com/wp-content/uploads/2019/10/shortcode-featured.png" width="744" height="988" alt="WooCommerce Shortcode - Featured Products" title="WooCommerce Shortcode – Featured Products" />
<h4 id="scenario-3-best-selling-products">Scenario 3 – Best Selling Products</h4>
I want to display my three top best selling products in one row.
<pre>[products limit="3" columns="3" best_selling="true" ]</pre>
<img class="alignnone size-full wp-image-3258" src="https://www.hss5.com/wp-content/uploads/2019/10/shortcode-bestselling.png" width="806" height="381" alt="WooCommerce Shortcode - Best Selling Products" title="WooCommerce Shortcode – Best Selling Products" />
<h4 id="scenario-4-newest-products">Scenario 4 – Newest Products</h4>
I want to display the newest products first – four products across one row. To accomplish this, we’ll use the Post ID (which is generated when the product page is created), along with the order and orderby command. Since you can’t see the Post ID from the frontend, the ID#s have been superimposed over the images.
<pre>[products limit="4" columns="4" orderby="id" order="DESC" visibility="visible"]</pre>
<img class="alignnone size-full wp-image-3259" src="https://www.hss5.com/wp-content/uploads/2019/10/shortcodes-newest.png" width="805" height="327" alt="WooCommerce Shortcodes - Newest" title="WooCommerce Shortcodes – Newest" />
<h4 id="scenario-5-specific-categories">Scenario 5 – Specific Categories</h4>
I only want to display hoodies and shirts, but not accessories. I’ll use two rows of four.
<pre>[products limit="8" columns="4" category="hoodies, tshirts" cat_operator="AND"]</pre>
<img class="alignnone size-full wp-image-3260" src="https://www.hss5.com/wp-content/uploads/2019/10/shortcode-categories1.png" width="811" height="733" alt="WooCommerce Shortcode - Products by Category" title="WooCommerce Shortcode – Products by Category" />

Alternatively, I only want to display products not in those categories. All I need to change is the <code>cat_operator</code> to <code>NOT IN</code>.
<pre>[products limit="8" columns="4" category="hoodies, tshirts" cat_operator="NOT IN"]</pre>
Note that even though the limit is set to <code>8</code>, there are only four products that fit that criteria, so four products are displayed.

<img class="alignnone size-full wp-image-3261" src="https://www.hss5.com/wp-content/uploads/2019/10/shortcode-categories2.png" width="798" height="360" alt="WooCommerce Shortcode - Products by Category" title="WooCommerce Shortcode – Products by Category" />
<h4 id="scenario-6-attribute-display">Scenario 6 – Attribute Display</h4>
Each of the clothing items has an attribute, either “Spring/Summer” or “Fall/Winter” depending on the appropriate season, with some accessories having both since they can be worn all year. In this example, I want three products per row, displaying all of the “Spring/Summer” items. That attribute slug is <code>season</code>, and the attributes are <code>warm</code> and <code>cold</code>. I also want them sorted from the newest products to the oldest.
<pre>[products columns="3" attribute="season" terms="warm" orderby="date"]</pre>
<img class="alignnone size-full wp-image-3262" src="https://www.hss5.com/wp-content/uploads/2019/10/shortcode-attribute1.png" width="815" height="886" alt="WooCommerce Shortcode - Products by Attribute" title="WooCommerce Shortcode – Products by Attribute" />

Alternatively, if I wanted to display exclusively cold weather products, I could add <code>NOT IN</code> as my <code>terms_operator</code>:
<pre>[products columns="3" attribute="season" terms="warm" terms_operator="NOT IN"]</pre>
<img class="alignnone size-full wp-image-3263" src="https://www.hss5.com/wp-content/uploads/2019/10/shortcode-attribute2.png" width="804" height="878" alt="WooCommerce Shortcode - Products by Attribute" title="WooCommerce Shortcode – Products by Attribute" />
Note that by using <code>NOT IN</code>, I exclude products that are both in “Spring/Summer” and “Fall/Winter”. If I wanted to show all cold-weather appropriate gear including these shared accessories, I would change the term from <code>warm</code> to <code>cold</code>.
<h3 id="section-9">Scenario 7 – Show Top Level Categories Only</h3>
Imagine you only wanted to show top level categories on a page and exclude the sub categories, well it’s possible with the following shortcode.
<pre>[product_categories number="0" parent="0"]</pre>
<img class="alignnone size-full wp-image-3264" src="https://www.hss5.com/wp-content/uploads/2019/10/woocommerce-shortcodes-top-level-categories-only.png" width="950" height="575" alt="" title="WooCommerce Shortcodes Top Level Categories Only" />
<h3 id="section-10">Scenario 8 – Show Only Products With tag “hoodie”</h3>
<pre>[products tag="hoodie"]</pre>
<img class="alignnone size-full wp-image-3265" src="https://www.hss5.com/wp-content/uploads/2019/10/screen-shot-2018-05-09-at-12-35-12.png" width="550" height="287" alt="" title="Screen Shot 2018-05-09 at 12.35.12" />
<h3 id="section-11">Sorting Products by Custom Meta Fields</h3>
When using the Products shortcode, you can choose to order products by the pre-defined values above. You can also sort products by custom meta fields using the code below (in this example we order product by price):
<pre>add_filter( 'woocommerce_shortcode_products_query', 'woocommerce_shortcode_products_orderby' );

function woocommerce_shortcode_products_orderby( $args ) {

    $standard_array = array('menu_order','title','date','rand','id');

    if( isset( $args['orderby'] ) &amp;&amp; !in_array( $args['orderby'], $standard_array ) ) {
        $args['meta_key'] = $args['orderby'];
        $args['orderby']  = 'meta_value_num'; 
    }

    return $args;
}
</pre>
You need to place this snippet in functions.php in your theme folder and then customize by editing the meta_key.
<div class="woo-sc-box note   "><b>Note:</b> We are unable to provide support for customizations under our <a href="http://woocommerce.com/support-policy/"><span class="s2">Support Policy</span></a>. If you are unfamiliar with code/templates and resolving potential conflicts, you can contact a <a href="https://woocommerce.com/wooexperts/"><span class="s2">WooExpert</span></a>.</div>
<h2 id="section-12">Product Page</h2>
Show a full single product page by ID or SKU.
<pre class="brush: php; gutter: false">[product_page id="99"]
[product_page sku="FOO"]</pre>
<h2 id="section-13">Related Products</h2>
List related products.

Args:
<pre>array(
     'limit' =&gt; '12',
     'columns' =&gt; '4',
     'orderby' =&gt; 'title'
 )

[related_products limit="12"]</pre>
<h2 id="section-14">‘limit’ Argument</h2>
<div class="woo-sc-box note   "><strong>Note:</strong> the ‘limit’ shortcode argument will determine how many products are shown on a page. This will not add pagination to the shortcode.</div>
<h2 id="section-15">Add to Cart</h2>
Show the price and add to cart button of a single product by ID.

Args:
<pre class="brush: php; gutter: false">array(
      'id' =&gt; '99',
      'style' =&gt; 'border:4px solid #ccc; padding: 12px;',
      'sku' =&gt; 'FOO'
      'show_price' =&gt; 'TRUE'
      'class' =&gt; 'CSS-CLASS'
      'quantity' =&gt; '1';
 )</pre>
<pre class="brush: php; gutter: false">[add_to_cart id="99"]</pre>
<h2 id="section-16">Add to Cart URL</h2>
Echo the URL on the add to cart button of a single product by ID.

Args:
<pre class="brush: php; gutter: false">array(
      'id' =&gt; '99',
      'sku' =&gt; 'FOO'
 )</pre>
<pre class="brush: php; gutter: false">[add_to_cart_url id="99"]</pre>
<h2 id="section-17">Display WooCommerce notifications on non-WooCommerce pages</h2>
<code>[shop_messages]</code> allows you to show WooCommerce notifications (like, ‘The product has been added to cart’) on non-WooCommerce pages. Helpful when you use other shortcodes, like <code>[add_to_cart]</code>, and would like the users to get some feedback on their actions.
<h2 id="section-18">Troubleshooting Shortcodes</h2>
If you correctly pasted your shortcodes and the display looks incorrect, make sure you did not embed the shortcode between <strong>&lt;pre&gt;</strong> tags. This is a common issue. To <strong>remove</strong> these tags, edit the page, and click the Text tab:

<img class="alignnone size-full wp-image-3266" src="https://www.hss5.com/wp-content/uploads/2019/10/WooCommerce-Shortcode-Pre-Tags.png" width="847" height="152" alt="Remove Pre Tags from Shortcode" />

</div>
<div class="related-docs docs-listing single">
<h2>Relevant Links</h2>
<ul class="recommended-docs">
 	<li>
<h4><a href="http://docs.woocommerce.com/documentation/plugins/woocommerce/getting-started/settings/">WooCommerce Settings</a></h4>
</li>
 	<li>
<h4><a href="http://docs.woocommerce.com/document/woocommerce-menu-items/">WooCommerce Menu</a></h4>
</li>
 	<li>
<h4><a href="http://docs.woocommerce.com/document/permalinks/">Permalinks</a></h4>
</li>
 	<li>
<h4><a href="http://docs.woocommerce.com/document/shop-currency/">Shop Currency</a></h4>
</li>
 	<li>
<h4><a href="http://docs.woocommerce.com/document/setting-up-taxes-in-woocommerce/">Setting up Taxes</a></h4>
</li>
 	<li>
<h4><a href="http://docs.woocommerce.com/document/woocommerce-localization/">Translating WooCommerce</a></h4>
</li>
 	<li>
<h4><a href="http://docs.woocommerce.com/document/woocommerce-widgets/">WooCommerce Widgets</a></h4>
</li>
 	<li>
<h4><a href="https://docs.woocommerce.com/document/how-to-get-help/">How to Get Help</a></h4>
</li>
</ul>
</div>