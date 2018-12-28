---
ID: 1063
post_title: >
  how to import multi-image to magento
  with magmi
author: chrispengcn
post_excerpt: |
  media_gallery (extra images) column syntax
  
  
  <image1 location>[::image1 label];<image2 location>[::image2 label]
  
  Example (with no labels):
  
  
  /extraimg1.jpg;/extraimg2.jpg
  
  Example (with labels):
  
  
  /extraimg1.jpg::label for img 1;/extraimg2.jpg::label for img 2
layout: post
permalink: >
  http://hss5.com/2017/09/28/how-to-import-multi-image-to-magento-with-magmi/
published: true
post_date: 2017-09-28 11:40:17
---
<h1 id="firstHeading" class="firstHeading" lang="en">Import Images</h1>
<div id="bodyContent" class="mw-body-content"></div>
Magmi supports both local image &amp; remote image import from urls. it also supports image gallery to associate multiple images with a product All image import is done by using <a title="Image attributes processor" href="http://wiki.magmi.org/index.php?title=Image_attributes_processor">Image attributes processor</a>.

&nbsp;

http://wiki.magmi.org/index.php?title=Image_attributes_processor

&nbsp;
<h1><span id="How_to_use" class="mw-headline">How to use</span></h1>
<h2><span id="Reminder" class="mw-headline">Reminder</span></h2>
<b>This plugin is Mandatory to handle images</b>
<h2><span id="Base_product_images_columns" class="mw-headline"><b>Base product images</b> columns</span></h2>
For compatibility with Dataflow legacy format
<h3><span id="To_set_base_image_values" class="mw-headline">To set base image values</span></h3>
<ul>
 	<li><b>image</b></li>
 	<li><b>small_image</b></li>
 	<li><b>thumbnail</b></li>
</ul>
<h3><span id="To_set_base_image_labels" class="mw-headline">To set base image labels</span></h3>
<ul>
 	<li><b>image_label</b> (label to use for image)</li>
 	<li><b>small_image_label</b> (label to use for small_image)</li>
 	<li><b>thumbnail_label</b> (label to use for thumbnail image)</li>
</ul>
<h2><span id="Extra_product_images" class="mw-headline"><b>Extra product images</b></span></h2>
The Extra product images are handled through <b>media_gallery</b> column

Since several images can be set in this column,a specific syntax has been set to handle labels.
<ul>
 	<li><b>The Extended syntax with :: for setting labels ONLY APPLIES TO media_gallery column</b></li>
 	<li><b>TO SET BASE IMAGE LABELS USE BASE IMAGE LABEL RELATED COLUMNS</b></li>
</ul>
<h3><span id="media_gallery_.28extra_images.29_column_syntax" class="mw-headline">media_gallery (extra images) column syntax</span></h3>
<pre><code>
 &lt;image1 location&gt;[::image1 label];&lt;image2 location&gt;[::image2 label]
</code></pre>
<h4><span id="Example_.28with_no_labels.29:" class="mw-headline">Example (with no labels):</span></h4>
<pre><code>
 /extraimg1.jpg;/extraimg2.jpg
</code></pre>
<h4><span id="Example_.28with_labels.29:" class="mw-headline">Example (with labels):</span></h4>
<pre><code>
 /extraimg1.jpg::label for img 1;/extraimg2.jpg::label for img 2
</code></pre>